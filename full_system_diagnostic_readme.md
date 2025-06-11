# 🩺 FullSystemDiagnostic.ps1 – MVP Documentation

A **portable PowerShell 5 script** that generates a comprehensive health‑check report for any Windows PC (desktop or laptop) in **one run and one file**. Designed for IT techs, refurbishers, and power users who need a quick PASS / WARN / FAIL snapshot plus detailed specs—without installing extra modules.

---

## ⚡️ What It Does

| Step | Action                                                                                    | Notes                                           |
| ---- | ----------------------------------------------------------------------------------------- | ----------------------------------------------- |
| 1    | Locates/creates `Desktop\System_Check`                                                    | Keeps generated files away from system folders. |
| 2    | Gathers identity & hardware data via **built‑in CIM/WMI**                                 | Works on any standard Win 10/11 install.        |
| 3    | *(Laptop only)* pulls Windows **battery‑report.html** and parses health % and cycle count | Flags poor batteries automatically.             |
| 4    | Calculates **PASS / WARN / FAIL** for: Battery, CPU, RAM, Storage, GPU, Network           | Thresholds are conservative and editable.       |
| 5    | Dumps extras: Power‑plan limits, Windows activation, BitLocker status, optional app list  | Good for license and security audits.           |
| 6    | Writes a single UTF‑8 text file `system‑diagnostic‑report.txt`                            | One artifact—easy to e‑mail or archive.         |

---

## 📂 Output Files

| File                           | Always? | Description                                      |
| ------------------------------ | ------- | ------------------------------------------------ |
| `system‑diagnostic‑report.txt` | Yes     | Full chronological log **+** scoreboard summary. |
| `battery‑report.html`          | Laptops | Raw Windows battery report for deeper analysis.  |

> **Typical size:** < 50 KB (without apps list) – safe for e‑mail attachments.

---

## 🛠 Installation / Usage

```powershell
# 1. Optional: bypass script restrictions (session‑local)
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass

# 2. Run (standard)
PowerShell -File .\FullSystemDiagnostic.ps1

# 3. Run with installed‑apps section (longer report)
PowerShell -File .\FullSystemDiagnostic.ps1 -IncludeApps
```

Run in an **elevated** console to ensure BitLocker and network queries succeed.

---

## 🎯 Scoring Logic (defaults)

| Component   | PASS                         | WARN                  | FAIL                 |
| ----------- | ---------------------------- | --------------------- | -------------------- |
| **Battery** | ≥ 60 % design capacity       | 40 – 59 %             | < 40 %               |
| **CPU**     | ≥ 4 physical cores           | 2–3 cores             | 1 core               |
| **RAM**     | ≥ 16 GB                      | 8–15.9 GB             | < 8 GB               |
| **Storage** | All disks `Healthy`          | —                     | Any disk `Unhealthy` |
| **GPU**     | Discrete NVIDIA/AMD detected | Intel/other iGPU only | —                    |
| **Network** | ≥ 1 NIC visible              | —                     | No NIC detected      |

> Adjust thresholds by editing the `Set‑Score` blocks inside the script.

---

## 🧩 Script Structure (high‑level)

```text
0  Helpers & scoreboard
1  System Identity
2  Battery Health (laptop only)
3  CPU Info & score
4  Memory Info & score
5  Storage Scan & score
6  GPU Detection & score
7  Touchscreen check (laptop)
8  Power‑plan limits dump
9  Windows Activation list
10 BitLocker status (drive C:)
11 Network Adapter inventory & score
12 Apps list (optional switch)
== Scoreboard summary ==
```

---

## ❓ FAQ

### Q 1. *Does it require PowerShell 7?*

No. It’s written for Windows‑built‑in **PowerShell 5.1**, so it runs on any standard Win 10/11 machine.

### Q 2. *How do I change the output folder?*

Edit the `$OutFolder` variable at the top of the script.

### Q 3. *Why are bullet symbols garbled in Notepad?*

The script now writes plain `*` bullets and forces UTF‑8 encoding, so Notepad and old editors show them correctly.

### Q 4. *Battery section missing on a desktop, is that an error?*

No. The script sets Battery = `N/A` when the chassis is not detected as a laptop.

---

## 🚧 Road‑map / Nice‑to‑haves

- SMART wear‑level & temperature read‑outs.
- Optional HTML report with colour highlights.
- CSV / JSON export for fleet inventory.
- Auto‑email of report via SMTP.

---

© 2025 – MIT‑licensed (template). Modify & redistribute freely.

