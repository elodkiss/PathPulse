# PathPulse v2.2
### Host Connectivity & RTT Monitor

## 📝 Description
PathPulse v2.2 is a high-performance command-line interface (CLI) tool designed for monitoring network connectivity and Round-Trip Time (RTT). It provides a lightweight and efficient way to track host availability and network performance in real-time.

> **💻 Platform Support:** This tool is designed exclusively for **Windows** (CMD/PowerShell).

> **⚙️ Requirement:** .NET Framework 4.8 (Included by default in modern Windows 10/11).

## ✨ Features
* **Windows Optimized:** Native performance on Windows network stacks.
* **Real-time RTT Monitoring:** Track network latency with high precision.
* **Dual-Language Help:** Includes full documentation in [English (`--help`)](./USAGE.md#english) and [Hungarian (`--helphu`)](./USAGE.md#hungarian).
* **Customizable Intervals:** Fully adjustable polling rates to suit your monitoring needs.
* **Statistical Summaries:** Gain insights through aggregated performance metrics.

## 🛠️ Installation & Usage
1. **Prerequisite:** Ensure **.NET Framework 4.8** is installed. 
   * *If the application fails to start, download the Runtime from Microsoft [here](https://dotnet.microsoft.com/en-us/download/dotnet-framework/net48).*
2. **Download** the latest `PathPulse.exe` from the [Releases](https://github.com/elodkiss/PathPulse/releases) section.
3. **Run** the application via Command Prompt or PowerShell:

```cmd
./PathPulse -target [IP/Host] -pingfreq [ms] -pingtimeout [ms]
./PathPulse -help
./PathPulse -helphu
```

### 🛡️ Security & Verification
To ensure the file is authentic and has not been tampered with, you can verify
the SHA-256 checksum below:

**SHA-256:** `1e7b0e2e7e3dfdc608dd20c60f8d1a100c3a17ac772b979315fc12b164f3e47b`

> **Note:** As this is a new release, Windows SmartScreen or Chrome might 
> flag it as "unrecognized". This is normal for indie software. 
> You can check the VirusTotal scan here: [Link to your scan]


## ⚖️ License
This software is closed-source. It is provided free of charge and may be used and redistributed freely in its original, unmodified executable form.

## 💡 Troubleshooting Tip: How to locate connection issues?

If PathPulse indicates high RTT or packet loss, follow these steps to pinpoint the source of the problem:

1.  **Identify the Path:** Use a traceroute tool (e.g., `tracert` or `traceroute`) to find the IP addresses of your first two hops.
2.  **Comparative Testing:** Run PathPulse on multiple targets simultaneously to isolate the fault:
    * **Hop 1:** Test your local router to check for **LAN/Wi-Fi** issues.
    * **Hop 2:** Test the first external gateway to check the **connection to your ISP**.
    * **External (8.8.8.8):** Test a global node (like Google) to see if the issue is with the **wider internet**.
3.  **AI Log Analysis:** Copy the output logs from these tests and paste them into **Gemini**. Use a prompt like: *"Compare these three PathPulse logs (Hop 1, Hop 2, Google). Based on the RTT patterns, is the bottleneck in my local network, the ISP, or the global routing?"*

## 🚀 Automation: Multi-target Batch Script
You can automate the monitoring of your entire network path using a Windows Batch file (`.bat`). Copy the following into a text file, save it as `monitor.bat` in the same folder as `PathPulse.exe`, and run it.

> **Note:** Replace the placeholder IP addresses with your actual gateway IPs. Also, replace `[HH:MM:SS]` with your desired start time, or remove the `--start` parameter to begin immediately.

```batch
@echo off
:: Replace [YOUR_ROUTER_IP] with your local gateway (e.g., 192.168.0.1) and set [HH:MM:SS]
start "" "PathPulse.exe" --target [YOUR_ROUTER_IP] --pingfreq 500 --pingtimeout 5 --procfreq 60000 --chart --events --quietevents --acceptlate --logpostfix _hop1_{target} --logbase {datetime}_{clientname} --start [HH:MM:SS] --timesync

:: Replace [ISP_GATEWAY_IP_1] with your ISP's first hop and set [HH:MM:SS]
start "" "PathPulse.exe" --target [ISP_GATEWAY_IP_1] --pingfreq 500 --pingtimeout 50 --procfreq 60000 --chart --events --quietevents --acceptlate --logpostfix _hop2_{target} --logbase {datetime}_{clientname} --start [HH:MM:SS] --timesync

:: Replace [ISP_GATEWAY_IP_2] with your ISP's second hop and set [HH:MM:SS]
start "" "PathPulse.exe" --target [ISP_GATEWAY_IP_2] --pingfreq 500 --pingtimeout 50 --procfreq 60000 --chart --events --quietevents --acceptlate --logpostfix _hop3_{target} --logbase {datetime}_{clientname} --start [HH:MM:SS] --timesync

:: Global Benchmark (Google DNS) - Set [HH:MM:SS] for synchronized start
start "" "PathPulse.exe" --target 8.8.8.8 --pingfreq 500 --pingtimeout 50 --procfreq 60000 --chart --events --quietevents --acceptlate --logpostfix _google_{target} --logbase {datetime}_{clientname} --start [HH:MM:SS] --timesync
