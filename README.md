
```markdown
# 🚀 Optimization Guide: Make Your PC Faster with Process Lasso & Tame Windows Defender
```
A minimalist, high-efficiency guide to optimizing your CPU performance, eliminating system micro-stutters, maximizing responsiveness, and taming Windows Defender (`Antimalware Service Executable` / `MsMpEng.exe`) using **Process Lasso** and built-in Windows PowerShell controls.


<br>

## ❓ What is Process Lasso & How Does It Help?

**Process Lasso** is an advanced process priority optimization and system automation utility. Unlike traditional task managers or "PC cleaners," it doesn't just kill processes—it intelligently manages how Windows allocates your CPU cores to active applications.

### 🌟 Is it good? Why use it?

* **Eliminates Micro-Stutters:** If a background task (like a Windows update, antivirus scan, or runaway browser tab) spikes your CPU, it can freeze your mouse cursor or cause lag spikes in video games, IDEs, and heavy applications. Process Lasso prevents this in real time.
* **Smart Resource Balancing:** It actively lowers the priority of background hogs only when they threaten to lag your active foreground window.
* **Extremely Lightweight:** The engine runs invisibly in the background using virtually **0% CPU** and a tiny sliver of RAM. It provides a massive performance benefit with zero resource overhead.

---

<br>

## 📥 Download & Installation

Process Lasso offers a premium tier, but **the Free Version is more than enough** for everyday users, gamers, and professionals alike. All core automation and optimization features are completely free forever.

### 🌐 Direct Download Link

* **Official Website:** [Download Process Lasso from Bitsum](https://bitsum.com/get-lasso-now/)

### ⚡ Quick PowerShell Installation

For a faster, automated setup without opening a browser, you can install Process Lasso via Windows Package Manager (**winget**) directly through PowerShell:

1. Right-click your Windows Start Menu and select **PowerShell** or **Terminal** (Run as Administrator).
2. Copy and paste the following command, then press **Enter**:

```powershell
winget install Bitsum.ProcessLasso
```

3. Follow any standard on-screen prompts to finish the installer.

---

<br>

## 🛠️ Step-by-Step Basic Setup (Optimal Configuration)

Open the Process Lasso user interface and follow these simple configurations to get the perfect balance of raw performance and battery efficiency.

### Step 1: Enable Core Optimizations

Go to the **Main** menu at the top of the interface and configure the following:

* **✅ Turn ON "ProBalance":** This is Process Lasso's flagship feature. It watches for rogue background processes and instantly scales down their priority to keep your foreground app perfectly smooth.
* **✅ Turn ON "SmartTrim":** A highly intelligent RAM management algorithm that gently cleans unneeded memory without causing performance hits.
* **❌ Leave "Performance Mode" OFF:** Do *not* forcefully lock your PC into maximum performance mode permanently. Doing so forces your processor to work constantly at its maximum frequency, creating an enormous amount of **excess heat** and rapidly **draining your battery**.
* **❌ Leave "Idle Saver" OFF:** Do *not* enable Idle Saver. While it saves power when your PC is idle, it downclocks your processor so heavily that if an app suddenly needs a quick performance boost, the system will lag or feel sluggish while trying to ramp back up.

<br>

### Step 2: Configure Your Active Power Profile

Also under the **Main** menu, look for the **Active Power Profile** submenu. Here you will find a list of power plans (like Balanced, High Performance, Bitsum Highest Performance, and Power Saver).

* **⚖️ Recommended (The Middle Ground):** For most situations, it is recommended to keep this setting right in the middle (such as a standard **Balanced** mode). Do not jump straight to extreme high performance or extreme battery saving right away.
* **🔥 Why avoid permanent Max/High Performance?** Forcing profiles like *High Performance* or *Bitsum Highest Performance* locks your processor at high clock speeds constantly. While you do get raw performance, it generates a massive amount of internal heat and consumes a lot of battery (or uses excessive wall energy if plugged in).
* **🐌 Why avoid extreme Battery Saving?** Selecting a highly restrictive battery profile does the exact opposite—it chokes your processor's speeds so heavily that the PC won't be able to provide a smooth performance boost when a heavy app actually needs it.
* **🔌 The Plugged-in Exception:** If your computer is normally kept **plugged into the wall**, you can safely select a **High Performance** profile since battery drain isn't an issue. However, if you regularly use your machine on **battery power**, keep it pinned to a **Balanced** profile to save power and lower heat.

<br>

### Step 3: Configure Silent & Automated Startup

To make Process Lasso work completely on autopilot without nag screens or consuming desktop resources, configure it to run as a native system service:

1. Click on the **Options** tab in the top menu bar.
2. Navigate to **General** ➡️ **Configure Startup...**
3. In the **Core Engine** section, select:
   * **👉 Start Core Engine as a service at System Boot**
   * *Why?* This allows the lightweight engine to boot instantly with Windows and handle all process priorities at a system level without requiring any manual interaction.
4. In the **Management Console (GUI) Startup** section, locate the dropdown (which defaults to "Startup at login for all users") and change it to:
   * **👉 Do not start at login**
   * *Why?* Choosing this prevents the user interface app from opening every time you sign in. This completely stops the periodic free-version popup prompt asking you to upgrade to the premium license, keeping your experience completely silent and uninterrupted.
5. Click **Next**, review the settings, and click **Finish/Save**.

> **⚠️ Important Note:** Once you finish saving these startup adjustments, you will need to **restart your PC** to fully apply the service-level changes and see the automatic configuration take effect.

---

<br>

## 🛡️ Taming Windows Defender (`MsMpEng.exe` / Antimalware Service)

Windows Defender is essential for system protection, but by default, it causes major performance bottlenecks for developers and everyday users:
- **Consumes up to 50% CPU** during routine background scans.
- **Infinite Scan Loops:** Scans its own database and engine files while scanning other files on your disk.
- **Developer IDE & App Lag:** Real-time scanning intercepts every single file read/write made by IDEs (Antigravity IDE, VS Code, Cursor, JetBrains), package managers (`npm`, `pip`, `cargo`), compiler builds, and Git operations.

---

### ⚡ Quick 1-Command Optimization (PowerShell)

Open **PowerShell (Run as Administrator)** and run:

```powershell
# 1. Cap maximum background scan CPU usage to 15% (Default is 50%)
Set-MpPreference -ScanAvgCPULoadFactor 15

# 2. Stop Defender from scanning its own engine process (Fixes disk I/O loops)
Add-MpPreference -ExclusionProcess "MsMpEng.exe"

# 3. Disable heavy archive (zip/rar) scanning during background idle scans
Set-MpPreference -DisableArchiveScanning $true

Write-Host "✅ Windows Defender resource limits successfully applied!" -ForegroundColor Green
```

#### What these commands do:
| Command | Effect | Why it matters |
| :--- | :--- | :--- |
| `-ScanAvgCPULoadFactor 15` | Capped at 15% CPU | Prevents Defender from making your PC freeze or fans spin up during background scans. |
| `-ExclusionProcess "MsMpEng.exe"` | Self-Scan Prevention | Stops Defender from continuously intercepting and scanning its own files in an endless disk loop. |
| `-DisableArchiveScanning $true` | Skip Archive Inspection | Prevents Defender from unzipping and deep-scanning massive archives in the background. |

---

### 🚀 Recommended Developer & IDE Exclusions

Whenever an IDE compiles code, writes logs, or indexes files, real-time protection checks every single file. Adding exclusions for your dev tools provides a **massive performance boost (up to 3x faster build times & indexing)**.

#### 1. Developer IDEs & Tools
Run in **PowerShell (Run as Administrator)** to exclude developer processes:

```powershell
# IDEs & Code Editors
Add-MpPreference -ExclusionProcess "Antigravity.exe"        # Antigravity
Add-MpPreference -ExclusionProcess "Antigravity IDE.exe"    # Antigravity IDE
Add-MpPreference -ExclusionProcess "Code.exe"               # VS Code
Add-MpPreference -ExclusionProcess "cursor.exe"             # Cursor IDE
Add-MpPreference -ExclusionProcess "idea64.exe"             # IntelliJ IDEA / PyCharm / WebStorm

# Language Servers, Git & Node Runtimes
Add-MpPreference -ExclusionProcess "node.exe"
Add-MpPreference -ExclusionProcess "git.exe"
Add-MpPreference -ExclusionProcess "language_server_windows_x64.exe"
```

#### 2. High-Traffic Developer Directories
Exclude folders that contain thousands of small cache files constantly created/deleted:

```powershell
# Exclude config & cache folders
Add-MpPreference -ExclusionPath "$env:USERPROFILE\.gemini"
Add-MpPreference -ExclusionPath "$env:USERPROFILE\.vscode"
Add-MpPreference -ExclusionPath "$env:LOCALAPPDATA\npm-cache"
Add-MpPreference -ExclusionPath "$env:LOCALAPPDATA\pnpm"
Add-MpPreference -ExclusionPath "$env:LOCALAPPDATA\Yarn"

# Optional: Add your active development workspaces
# Add-MpPreference -ExclusionPath "C:\Projects"
# Add-MpPreference -ExclusionPath "$env:USERPROFILE\Downloads\my site"
```

---

<br>

## 🎛️ Real-Time CPU Limiting via Process Lasso (CPU Sets vs. Affinity)

Because `MsMpEng.exe` is protected by Windows kernel security (**Protected Process Light / PPL**), standard task managers cannot easily change its core assignment in real-time. 

Here is how to properly restrict Windows Defender to specific CPU cores without triggering Windows security blocks.

---

### 🔍 Step 1: Check How Many CPU Cores & Threads You Have

Before assigning cores, check your exact CPU core/thread layout by running this quick snippet in **PowerShell**:

```powershell
$cpu = Get-CimInstance Win32_Processor
Write-Host "================ CPU INFORMATION ================" -ForegroundColor Cyan
Write-Host "Processor:        $($cpu.Name)"
Write-Host "Physical Cores:   $($cpu.NumberOfCores)"
Write-Host "Logical Threads:  $($cpu.NumberOfLogicalProcessors)"
Write-Host "Available IDs:    0 to $($cpu.NumberOfLogicalProcessors - 1)"
Write-Host "=================================================" -ForegroundColor Cyan
```

* **Example Output:**
  * If Logical Threads is **4**, your CPU IDs are **`CPU 0, CPU 1, CPU 2, CPU 3`**.
  * If Logical Threads is **8**, your CPU IDs are **`CPU 0` through `CPU 7`**.
  * If Logical Threads is **16**, your CPU IDs are **`CPU 0` through `CPU 15`**.

---

### 🧠 Step 2: Understanding "CPU Affinity" vs. "CPU Sets"

When right-clicking a process in Process Lasso, you will see both **CPU Affinity** and **CPU Sets**. Understanding the difference is crucial for Windows Defender:

| Feature | **CPU Affinity** (Legacy Method) | **CPU Sets** (Modern Windows 10/11 Method) |
| :--- | :--- | :--- |
| **How it works** | **Hard Hardware Lock:** Forces a process to only run on the chosen cores. If those cores are busy, the process freezes/waits. | **Soft Cooperative Assignment:** Tells the Windows Kernel Scheduler to prefer and restrict threads to those specific cores. |
| **Protected Processes (`MsMpEng.exe` / PPL)** | ❌ **Blocked by Windows Kernel.** Windows Defender rejects CPU Affinity requests, which is why changing affinity shows `0-3` (all cores) or Access Denied. | ✅ **Allowed by Windows Kernel.** CPU Sets works seamlessly with protected system processes like Defender. |
| **System Latency** | Can cause micro-stutters if a system thread is hard-locked to a congested core. | **Zero micro-stutters.** Smooth thread scheduling while isolating the process from your main foreground cores. |

#### ❓ Why does "Current" look grayed out / default, but "Always" works?
* When clicking **Current**, Process Lasso makes an immediate standard Windows API call. Because `MsMpEng.exe` is protected by Windows PPL, Windows rejects the immediate user-mode call.
* When configuring under **Always**, Process Lasso’s background governor service (`ProcessGovernor.exe`) enforces the rule directly at the system level upon process execution. **Always is the correct place to set your rules.**

---

### 🎯 Step 3: Which Cores Should You Select?

> **⚠️ Golden Rule: NEVER assign heavy background tasks to `CPU 0`!**
> `CPU 0` (Core 0) is the most critical core in Windows. The operating system uses `CPU 0` for system interrupts (DPCs/ISRs), mouse and keyboard input, audio drivers, and graphics rendering. Putting Defender on `CPU 0` causes mouse and audio stutters.

Use the table below to choose the right cores based on your CPU thread count:

| Total Logical Threads | Cores Reserved for UI / IDE / Games | Cores to Select for Defender (`MsMpEng.exe`) |
| :---: | :---: | :---: |
| **4 Threads** (e.g. 2C/4T like i5-7200U) | `CPU 0, CPU 1` | **`CPU 2, CPU 3`** (or just `CPU 3`) |
| **8 Threads** (e.g. 4C/8T or 8C/8T) | `CPU 0, CPU 1, CPU 2, CPU 3` | **`CPU 6, CPU 7`** (or `CPU 4, 5, 6, 7`) |
| **12 Threads** (e.g. 6C/12T) | `CPU 0` through `CPU 7` | **`CPU 10, CPU 11`** |
| **16 Threads** (e.g. 8C/16T) | `CPU 0` through `CPU 11` | **`CPU 14, CPU 15`** |

---

### ⚙️ Step 4: Step-by-Step Configuration in Process Lasso

1. Open **Process Lasso** (Run as Administrator).
2. In the search/filter box at the top, type `MsMpEng.exe`.
3. Right-click on **`MsMpEng.exe`** and configure the following:
   * **CPU Sets:**
     - Select **CPU Sets** ➔ **Always** ➔ **Select CPU Sets...**
     - Check only your target background cores (for a 4-thread system, check **`CPU 2` and `CPU 3`**).
     - Click **OK**.
   * **CPU Priority:**
     - Select **CPU Priority** ➔ **Always** ➔ **Below Normal** (or **Normal**).
   * **I/O Priority:**
     - Select **I/O Priority** ➔ **Always** ➔ **Low** (or **Below Normal**).

> 💡 **Result:** Even when Windows Defender initiates a heavy real-time virus scan, it will be strictly confined to your secondary cores, leaving `CPU 0` and your primary cores 100% free and responsive!

---

<br>

## 🔄 How to Check or Revert Changes

### View Current Preferences & Exclusions:
```powershell
# View active exclusions and CPU limits
$mp = Get-MpPreference
Write-Host "CPU Limit: $($mp.ScanAvgCPULoadFactor)%`n"
Write-Host "--- Excluded Processes ---" -ForegroundColor Cyan
$mp.ExclusionProcess | ForEach-Object { Write-Host " • $_" }
Write-Host "`n--- Excluded Paths ---" -ForegroundColor Cyan
$mp.ExclusionPath | ForEach-Object { Write-Host " • $_" }

```

### Revert Defender Settings to Default:
```powershell
# Reset CPU load factor back to default (50%)
Set-MpPreference -ScanAvgCPULoadFactor 50

# Remove an exclusion if needed
Remove-MpPreference -ExclusionProcess "MsMpEng.exe"
```

### How to Remove a specific process exclusion:
```powershell
Remove-MpPreference -ExclusionProcess "node.exe"
```

### How to Remove a specific folder path exclusion:
```powershell
Remove-MpPreference -ExclusionPath "$env:LOCALAPPDATA\npm-cache"
```

### How to Remove ALL Exclusions & Reset Defender :
```powershell
$mp = Get-MpPreference

# 1. Clear All Process Exclusions
if ($mp.ExclusionProcess) {
    $mp.ExclusionProcess | ForEach-Object {
        Remove-MpPreference -ExclusionProcess $_
        Write-Host "🗑️ Removed Process: $_" -ForegroundColor Yellow
    }
}

# 2. Clear All Path Exclusions
if ($mp.ExclusionPath) {
    $mp.ExclusionPath | ForEach-Object {
        Remove-MpPreference -ExclusionPath $_
        Write-Host "🗑️ Removed Path: $_" -ForegroundColor Yellow
    }
}

# 3. Reset Background Scan CPU Limit to Default (50%)
Set-MpPreference -ScanAvgCPULoadFactor 50

# 4. Re-enable Archive Scanning
Set-MpPreference -DisableArchiveScanning $false

Write-Host "`n✅ All Defender exclusions cleared & settings restored to factory defaults!" -ForegroundColor Green

```
---

<br>

## 📚 Advanced Customization & Documentation

There are many additional features in Process Lasso (such as Watchdog automation rules and custom I/O priority management). If you are interested in exploring the full documentation, check the official manual below:

🔗 **Official Knowledge Base & Manual:** [Bitsum Process Lasso Documentation](https://bitsum.com/docs/processlasso/)

---

<br>

## 📜 License
MIT License. Free for personal and commercial use.
```
