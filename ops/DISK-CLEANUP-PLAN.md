# Disk Cleanup Plan - C: Drive (87.4% Full)

> Handoff document. This file was prepared by Opus after researching the issue and scanning the disk. The next agent (Sonnet) should pick up from Phase 1 and execute step by step, asking the user before any destructive action.

---

## 1. Current State (verified at handoff)

| Metric | Value |
|---|---|
| Drive | C: |
| Total size | 375.9 GB (approx 403.6 GB raw) |
| Used | 328.6 GB |
| Free | 47.3 GB (~50.8 GB raw) |
| Usage | 87.4 % |
| OS | Windows 11 (build 10.0.26200) |
| User | PGI |

Goal: bring usage below 75 % (free up >= 50 GB), ideally to 65 % (free up >= 85 GB).

---

## 2. Top Space Consumers (measured)

### C:\Users\PGI\AppData\Local - 168.75 GB

| Folder | Size | What it is | Safe to clean? |
|---|---|---|---|
| Docker | 66.36 GB | Docker Desktop / WSL2 images, containers, volumes | YES - docker system prune |
| Programs | 57.81 GB | User-installed apps (VS Code, Cursor, etc.) | AUDIT - uninstall unused only |
| JetBrains | 16.24 GB | PhpStorm/IntelliJ caches, indexes, logs | YES - caches/logs only |
| Google | 5.30 GB | Chrome user data + cache | YES - cache only |
| npm-cache | 4.13 GB | npm download cache | YES - npm cache clean --force |
| Android | 3.84 GB | Android SDK / AVDs | AUDIT - only if not used |
| ms-playwright | 2.39 GB | Playwright browser binaries | AUDIT - only if not used |
| CapCut | 2.36 GB | CapCut cache | YES - cache only |
| Packages | 1.87 GB | UWP app data | SELECTIVE |
| pnpm | 1.70 GB | pnpm store | YES - pnpm store prune |
| Temp | 1.48 GB | User temp files | YES |
| Postman | 0.93 GB | Postman cache | YES - cache only |

### C:\Users\PGI\AppData\Roaming - 25.59 GB

| Folder | Size | Notes | Safe to clean? |
|---|---|---|---|
| Claude | 13.75 GB | Claude Desktop logs / cache / model files | LOGS+CACHE yes, conversations no |
| JetBrains | 8.72 GB | IDE config + plugins | PLUGINS+LOGS only |
| Cursor | 1.67 GB | Cursor logs/cache | YES - cache + logs only |
| Postman | 0.58 GB | Postman data | SELECTIVE |

### Other large user folders

| Path | Size |
|---|---|
| C:\Users\PGI\Downloads | 13.57 GB |
| C:\Users\PGI\.config | 5.56 GB |
| C:\Users\PGI\.local | 2.63 GB |
| C:\Users\PGI\.cursor | 2.17 GB |

---

## 3. Cleanup Phases - Run in Order

> Safety rules for executor (Sonnet):
> 1. Read this whole file first.
> 2. Confirm with the user before each phase.
> 3. Run each command, capture output, then re-measure free space.
> 4. NEVER delete from Documents, Desktop, OneDrive, Pictures, source-code folders, or anything in C:\Users\PGI\PhpstormProjects.
> 5. If a step fails or is risky, stop and report.

---

### Phase 1 - Zero-risk wins (target: ~7-10 GB)

1.1 Empty Recycle Bin

    powershell -NoProfile -Command "Clear-RecycleBin -Force -ErrorAction SilentlyContinue"

1.2 Clear user Temp + Windows Temp

    powershell -NoProfile -Command "Get-ChildItem -Path $env:TEMP -Recurse -Force -ErrorAction SilentlyContinue | Remove-Item -Recurse -Force -ErrorAction SilentlyContinue"
    powershell -NoProfile -Command "Get-ChildItem -Path 'C:\Windows\Temp' -Recurse -Force -ErrorAction SilentlyContinue | Remove-Item -Recurse -Force -ErrorAction SilentlyContinue"

1.3 Clear package-manager caches (run only the ones the user has)

    npm cache clean --force
    pnpm store prune
    yarn cache clean
    pip cache purge

1.4 Run Storage Sense (modern Windows cleanup)
- Open Settings -> System -> Storage -> Temporary files
- Tick: Temporary files, Delivery Optimization Files, Thumbnails, Recycle Bin, Windows Update Cleanup
- Click Remove files.

1.5 Run legacy Disk Cleanup as admin

    cleanmgr /d C:

Then click "Clean up system files" -> tick everything except Downloads.

Re-measure free space. Expected gain: 5-10 GB.

---

### Phase 2 - Docker cleanup (target: ~30-50 GB)

> Biggest single win. Docker Desktop hoards images/volumes/build cache.

2.1 Make sure Docker Desktop is running, then:

    docker system df

Show output to user - they decide.

2.2 Safe prune (keeps anything currently used by running containers):

    docker system prune -a -f
    docker volume prune -f
    docker builder prune -a -f

2.3 If user wants aggressive WSL2 reclaim (Docker uses a single VHDX):
- Stop Docker Desktop.
- Run:

    wsl --shutdown
    Optimize-VHD -Path "$env:LOCALAPPDATA\Docker\wsl\disk\docker_data.vhdx" -Mode Full

> If Optimize-VHD is missing, install Hyper-V PowerShell module:
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Management-PowerShell -All
> Or use Docker Desktop -> Settings -> Resources -> Disk image size -> "Clean / Purge data".

Re-measure free space. Expected gain: 20-40 GB.

---

### Phase 3 - IDE and app caches (target: ~10-15 GB)

3.1 JetBrains caches/logs (keeps settings, removes regenerable caches)

    $jb = "$env:LOCALAPPDATA\JetBrains"
    Get-ChildItem $jb -Directory | ForEach-Object {
      foreach ($sub in 'caches','log','tmp') {
        $p = Join-Path $_.FullName $sub
        if (Test-Path $p) { Remove-Item $p -Recurse -Force -ErrorAction SilentlyContinue }
      }
    }

3.2 Cursor / VS Code logs + caches

    Remove-Item "$env:APPDATA\Cursor\logs" -Recurse -Force -ErrorAction SilentlyContinue
    Remove-Item "$env:APPDATA\Cursor\Cache" -Recurse -Force -ErrorAction SilentlyContinue
    Remove-Item "$env:APPDATA\Cursor\CachedData" -Recurse -Force -ErrorAction SilentlyContinue
    Remove-Item "$env:APPDATA\Code\logs" -Recurse -Force -ErrorAction SilentlyContinue
    Remove-Item "$env:APPDATA\Code\CachedData" -Recurse -Force -ErrorAction SilentlyContinue

3.3 Claude Desktop cache/logs (13.75 GB folder - investigate before deleting)

    Get-ChildItem "$env:APPDATA\Claude" -Directory | ForEach-Object {
      $size = (Get-ChildItem $_.FullName -Recurse -Force -EA SilentlyContinue | Measure-Object Length -Sum).Sum
      [PSCustomObject]@{ Path=$_.FullName; SizeGB=[math]::Round($size/1GB,2) }
    } | Sort-Object SizeGB -Descending | Format-Table -AutoSize

- Delete ONLY Cache, Code Cache, GPUCache, logs, Service Worker\CacheStorage subfolders.
- DO NOT delete Local Storage, IndexedDB, or anything resembling user data without confirming.

3.4 Chrome / Brave caches

    Remove-Item "$env:LOCALAPPDATA\Google\Chrome\User Data\Default\Cache" -Recurse -Force -ErrorAction SilentlyContinue
    Remove-Item "$env:LOCALAPPDATA\Google\Chrome\User Data\Default\Code Cache" -Recurse -Force -ErrorAction SilentlyContinue
    Remove-Item "$env:LOCALAPPDATA\BraveSoftware\Brave-Browser\User Data\Default\Cache" -Recurse -Force -ErrorAction SilentlyContinue

3.5 CapCut / Postman cache

    Remove-Item "$env:LOCALAPPDATA\CapCut\User Data\Cache" -Recurse -Force -ErrorAction SilentlyContinue
    Remove-Item "$env:LOCALAPPDATA\Postman\Cache" -Recurse -Force -ErrorAction SilentlyContinue

3.6 Windows Update leftovers

    Stop-Service wuauserv -Force
    Remove-Item 'C:\Windows\SoftwareDistribution\Download\*' -Recurse -Force -ErrorAction SilentlyContinue
    Start-Service wuauserv

3.7 Component store cleanup (admin)

    Dism.exe /online /Cleanup-Image /StartComponentCleanup

Re-measure free space. Expected gain: 8-15 GB.

---

### Phase 4 - User audit (target: ~5-15 GB)

> Show user the list, let them decide.

4.1 List Downloads files larger than 100 MB

    Get-ChildItem "$env:USERPROFILE\Downloads" -Recurse -File -Force -EA SilentlyContinue |
      Where-Object Length -gt 100MB |
      Sort-Object Length -Descending |
      Select-Object FullName, @{N='SizeMB';E={[math]::Round($_.Length/1MB,1)}} |
      Format-Table -AutoSize

4.2 Find installer files in Downloads

    Get-ChildItem "$env:USERPROFILE\Downloads" -Include *.iso,*.msi,*.exe -Recurse -File -EA SilentlyContinue |
      Sort-Object Length -Descending |
      Select-Object FullName, @{N='SizeMB';E={[math]::Round($_.Length/1MB,1)}} |
      Format-Table -AutoSize

4.3 Audit AppData\Local\Programs (57 GB - unused dev tools?)

    Get-ChildItem "$env:LOCALAPPDATA\Programs" -Directory | ForEach-Object {
      $s = (Get-ChildItem $_.FullName -Recurse -Force -EA SilentlyContinue | Measure-Object Length -Sum).Sum
      [PSCustomObject]@{ Name=$_.Name; SizeGB=[math]::Round($s/1GB,2) }
    } | Sort-Object SizeGB -Descending | Format-Table -AutoSize

Use Settings -> Apps -> Installed apps, sort by size, uninstall what user confirms.

---

### Phase 5 - Optional / advanced (target: variable)

> Only if user asks and after Phases 1-4.

5.1 Disable Hibernation (frees hiberfil.sys, often several GB)

    powercfg -h off

5.2 Reduce System Restore size

    vssadmin resize shadowstorage /for=C: /on=C: /maxsize=5GB

5.3 Delete old Windows installation (C:\Windows.old, if present - only within 10 days of upgrade)
- Use Disk Cleanup -> "Previous Windows installation(s)".

5.4 Move OneDrive / Documents / Downloads to a different drive (if user has a D:).

5.5 Compress NTFS on rarely-used folders

    compact /c /s /a /i "C:\Users\PGI\Downloads\*"

---

## 4. Verification Snippet (run after each phase)

    powershell -NoProfile -Command "$v = Get-Volume -DriveLetter C; '{0:N2} GB free of {1:N2} GB ({2:N1}% used)' -f ($v.SizeRemaining/1GB), ($v.Size/1GB), ((1 - $v.SizeRemaining/$v.Size)*100)"

---

## 5. Final Report Template (Sonnet should output this when done)

    === Cleanup Summary ===
    Before:   328.6 GB used / 47.3 GB free  (87.4%)
    After:    <X> GB used / <Y> GB free  (<Z>%)
    Reclaimed: <N> GB

    Phase 1 (temp/cache):     +<a> GB
    Phase 2 (Docker):         +<b> GB
    Phase 3 (IDE caches):     +<c> GB
    Phase 4 (user audit):     +<d> GB
    Phase 5 (optional):       +<e> GB

    Skipped / declined: <list>
    Issues encountered: <list>
    Recommended next step: <e.g., enable Storage Sense schedule>

---

## 6. Long-term hygiene (recommend to user at the end)

- Settings -> System -> Storage -> Storage Sense -> On
  - Run every week, delete temp files, empty recycle bin after 30 days, clear Downloads after 60 days.
- Set Docker Desktop Disk image size limit (Settings -> Resources).
- Schedule monthly: docker system prune -a -f (after confirming no critical state).
- Keep Downloads under 5 GB; archive installers off-drive.
- Consider moving Docker, JetBrains caches, and Programs to a secondary drive via symbolic links if user has D:.

---

## 7. Notes for Sonnet (executor)

- Model recommendation for this task: claude-4.6-sonnet-medium-thinking (careful, methodical).
- Use composer-2-fast only for the simple Phase 1 commands.
- ALWAYS confirm before deleting from anywhere outside *\Cache, *\logs, *\Temp, *\node_modules, *\__pycache__.
- After every phase, print the verification snippet output.
- If free space crosses below 5 GB at any point, stop immediately and alert the user.

---

Prepared: 2026-05-07. Re-measure before executing - values may have drifted.
