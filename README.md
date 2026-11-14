# Digital Forensics Practical Laboratory Report

| Field | Details |
| :--- | :--- |
| **Course Code** | CIP-B101 |
| **Course Title** | Basic Computer Skills for Digital Forensics |
| **Laboratory Title** | Lab 1C: Windows Command Line for Digital Forensics Basics |
| **Student Name** | `OMOWUMI OMOLOLA SHARON` |
| **Registration Number** | `FDCF2617050` |
| **Date of Submission** | `13TH, November, 2026` |
| **Submission Package** | `CIP-B101_Lab1C_SHARON_OMOWUMI` |

---

## Table of Contents
1. [Objective](#1-objective)
2. [Laboratory Environment](#2-laboratory-environment)
3. [Methodology](#3-methodology)
4. [Practical Execution & Evidence Screenshots](#4-practical-execution--evidence-screenshots)
5. [Summary of Collected System & Network Outputs](#5-summary-of-collected-system--network-outputs)
6. [Mini Forensic Interpretation Questions (Task 12)](#6-mini-forensic-interpretation-questions-task-12)
7. [Conclusion](#7-conclusion)

---

## 1. Objective
The objective of this laboratory exercise is to build foundational Windows Command Prompt (`cmd.exe`) proficiency for digital forensic triage and evidence handling. Specifically, this practical demonstrates how to establish an isolated workspace, navigate the Windows file system using absolute and relative paths, manage files safely without cross-contamination, collect host and network configuration details, inspect active processes and file permissions, search text logs, and automate repeatable evidence collection using batch scripts and the `PATH` environment variable.

---

## 2. Laboratory Environment
All practical activities were performed in an isolated virtual environment to prevent unintended modifications to the host operating system.

* **Operating System:** Microsoft Windows 11 `[Insert Edition, e.g., Pro / Home — Build XXXXX]`
* **Virtualization Platform:** Oracle VM VirtualBox
* **User Privilege Level:** `[Standard User / Administrator]`
* **Tools Utilized:** Windows Command Prompt (`cmd.exe`), Notepad, Microsoft Edge
* **Designated Lab Workspace:** `C:\Users\Public\CIP-B101_Lab1C_[FIRSTNAME_LASTNAME]`

---

## 3. Methodology
To maintain forensic integrity and repeatability, the examination followed a structured command-line workflow:

1. **Workspace Isolation:** Created a dedicated directory structure under `C:\Users\Public` to segregate generated evidence, scripts, screenshots, and outputs from core operating system files.
2. **File System Navigation & Manipulation:** Used `cd`, `dir`, `tree`, `copy`, `xcopy`, `ren`, `del`, and `rmdir` to verify directory hierarchies and practice safe file handling.
3. **Host & Network Triage:** Executed `systeminfo`, `wmic`, `ipconfig`, and `ping` to extract operating system specifications, hardware inventory, network interfaces, MAC addresses, and connectivity status.
4. **Artifact Searching & Access Control Review:** Applied `findstr` to locate forensic indicators inside log files and used `dir /q` alongside `icacls` to verify file ownership and Access Control Lists (ACLs).
5. **Process Inspection & Automation:** Queried running processes via `wmic process` and developed two batch scripts (`hello_world.bat` and `sys_info.bat`) executed both locally and globally via a temporary `PATH` modification.

---

## 4. Practical Execution & Evidence Screenshots

### 4.1 Preparation: Laboratory Workspace Setup
A dedicated root folder (`CIP-B101_Lab1C_[FIRSTNAME_LASTNAME]`) and eight subfolders (`evidence`, `screenshots`, `outputs`, `scripts`, `workspace`, `workspace2`, `workspace3`, `workspace4`) were created under `C:\Users\Public` to organize all forensic artifacts cleanly.

## Mini Forensic Interpretation Questions (Task 12)

### 1. What is the difference between an absolute path and a relative path? Give one example of each from your lab.
An **absolute path** specifies the complete location of a file or folder starting from the root drive letter (`C:\`), regardless of the user's current working directory. A **relative path** specifies a file or folder location in relation to the current working directory without listing the full route from the drive root.

* **Absolute path example:** `C:\Users\Public\CIP-B101_Lab1C_[FIRSTNAME_LASTNAME]\outputs`
* **Relative path example:** `..\workspace2\evidence_note_copy.txt`

---

### 2. Why should analysts create a separate lab/evidence folder before running commands?
Creating an isolated workspace prevents accidental overwriting, corruption, or deletion of native operating system files. It also keeps all collected artifacts organized in one verifiable location and maintains proper evidence-handling hygiene without cross-contamination.

---

### 3. Which command showed your IP address and which command showed your MAC address?
* **IP Address:** `ipconfig` (as well as `ipconfig /all`) displayed the system's IPv4 address.
* **MAC Address:** `wmic nic get macaddress,description` (as well as `ipconfig /all`) displayed the physical MAC address of the network adapter.

---

### 4. Why is a batch file useful in forensic triage?
A batch file automates a predefined sequence of commands so analysts can collect volatile and non-volatile system data rapidly, consistently, and repeatably across multiple systems while minimizing manual typing errors.

---

### 5. Why must analysts document failed commands or failed pings instead of hiding them?
Accurate forensic documentation requires recording the exact state of the system as observed. A failed ping or command provides critical investigative context—such as proving that a host was network-isolated, blocked by a firewall, or offline—and preserves the integrity and credibility of the analyst's report.

---

### 6. What risk can occur if a suspicious executable is placed in a directory listed in `PATH`?
When a command is run without a full path, Windows searches directories listed in `PATH` sequentially. If a malicious executable shares the name of a legitimate utility and sits in a `PATH` directory searched before `System32`, Windows may execute the attacker's binary instead of the trusted system tool (**PATH hijacking / binary planting**), leading to unauthorized code execution or privilege escalation.

---

## Conclusion
Through this laboratory exercise, foundational Windows command-line skills essential for digital forensic triage were practiced within a controlled Windows 11 VirtualBox environment. By navigating directory structures, safely managing files, extracting system, network, and process metadata, inspecting file permissions, and building an automated batch script (`sys_info.bat`), this lab demonstrated how disciplined command-line workflows produce reliable, repeatable, and well-documented forensic evidence.

> *Figure 1: Verification of the isolated laboratory directory structure under `C:\Users\Public` using `mkdir` and `dir`.*

---

### 4.2 Task 1: Windows Paths and Navigation
Directory traversal was tested using root (`cd \`), absolute (`cd \Users`), and parent-relative (`cd ..`) path commands, followed by `tree` to map the workspace hierarchy.

* **Absolute Path Definition:** An absolute path specifies the complete location of a file or folder starting from the root drive letter (for example, `C:\Users\Public\CIP-B101_Lab1C_[FIRSTNAME_LASTNAME]`).
* **Relative Path Definition:** A relative path specifies a file or folder location in relation to the user's current working directory without listing the full path from the drive root (for example, `cd ..` or `Public\CIP-B101_Lab1C_[FIRSTNAME_LASTNAME]`).

![Figure 2: Windows Path Navigation and Tree Output](screenshots/02_path_navigation.png)
> *Figure 2: Command Prompt output demonstrating current directory inspection (`cd`), directory listing (`dir`), and folder tree mapping (`tree`).*

---

### 4.3 Task 2: Basic File and Folder Creation
Two text files (`my_first_file.txt` and `evidence_note.txt`) were created inside the `workspace` folder using standard output redirection, and their contents were verified using `type`.

* **Difference Between `>` and `>>`:** The single greater-than operator (`>`) creates a new file or completely overwrites an existing file's contents. The double greater-than operator (`>>`) appends new output to the end of an existing file without erasing previously stored data.

![Figure 3: File Creation and Verification](screenshots/03_file_creation.png)
> *Figure 3: Creation and content verification of `my_first_file.txt` and `evidence_note.txt` using `echo`, `>`, `>>`, and `type`.*

---

### 4.4 Task 3: Copy, Duplicate, Rename, and Safe Deletion
Evidence handling discipline was practiced by copying `evidence_note.txt` to `workspace2`, performing a recursive backup of `workspace` into `workspace3\workspace_backup` using `xcopy /E /I /H`, renaming the copied file to `renamed_evidence_note.txt`, and safely deleting only a temporary file (`delete_me.txt`) and an empty test directory (`empty_test_folder`).

![Figure 4: File Copy, Rename, and Safe Deletion](screenshots/04_copy_rename_delete.png)
> *Figure 4: Verification of recursive folder backup (`xcopy`), file renaming (`ren`), and selective deletion (`del`, `rmdir`) of temporary test items.*

---

### 4.5 Task 4: Understanding How Windows Finds Applications (`PATH`)
The active `PATH` environment variable was inspected with `echo %PATH%`, `where notepad` confirmed the executable resolves to `C:\Windows\System32\notepad.exe`, and `vol C:` verified the volume serial number.

* **Why `PATH` Matters in Malware Triage:** When a command is entered without a full path, Windows searches directories listed in `PATH` sequentially. If an attacker or malware places a malicious executable inside a writable directory listed early in `PATH`, Windows can execute the malicious binary instead of the legitimate system tool (known as PATH hijacking or binary planting).

![Figure 5: Environment PATH Inspection and Executable Resolution](screenshots/05_path_and_where.png)
> *Figure 5: Inspection of Windows `PATH` directories and resolution of `notepad.exe` via `where notepad`.*

---

### 4.6 Task 5: Collecting Windows System Information
Host operating system, memory, CPU, and virtual disk details were captured into `systeminfo_full.txt`, `cpu_info.txt`, and `disk_info.txt` inside the `outputs` directory.

![Figure 6: System, CPU, and Disk Information](screenshots/06_system_info.png)
> *Figure 6: Extraction of OS version, physical memory, processor model, and VirtualBox disk specifications using `systeminfo` and `wmic`.*

---

### 4.7 Task 6: Collecting Basic Network Information
Network adapter settings, IPv4 address, default gateway, and MAC addresses were exported to `ipconfig_all.txt` and `mac_addresses.txt`, followed by loopback (`127.0.0.1`) and external (`8.8.8.8`, `google.com`) connectivity checks.

* **Connectivity Note:** `[Edit as applicable: Loopback and external pings succeeded via VirtualBox NAT networking. / Loopback ping succeeded, while external pings failed because the VirtualBox VM network adapter was configured in an isolated Host-Only/Internal mode without external internet routing.]`

![Figure 7: Network Configuration, MAC Address, and Ping Checks](screenshots/07_network_info.png)
> *Figure 7: Output of IPv4 configuration, VirtualBox adapter MAC address, and ICMP `ping` connectivity tests.*

---

### 4.8 Task 7: Searching for Evidence Text in Files (`findstr`)
A simulated case log (`mini_case_log.txt`) was created in the `evidence` directory and queried using `findstr` to locate specific indicators.

* **Function of `/i` and `/n` in `findstr`:** The `/i` switch performs a case-insensitive search so uppercase and lowercase variations both match. The `/n` switch prints the exact line number before each matching line in the file.

![Figure 8: File Content Searching with findstr](screenshots/08_findstr_search.png)
> *Figure 8: Creation of `mini_case_log.txt` and keyword searches using `findstr /i` and `findstr /n /i`.*

---

### 4.9 Task 8: Checking File Ownership and Permissions
Ownership and Access Control List (ACL) permissions for `mini_case_log.txt` were examined using `dir /q` and `icacls`.

* **File Ownership:** On this workstation, `mini_case_log.txt` is owned by `[INSERT OWNER SHOWN IN dir /q, e.g., DESKTOP-XXXX\User or BUILTIN\Administrators]`.

![Figure 9: File Ownership and Permissions Review](screenshots/09_ownership_permissions.png)
> *Figure 9: Verification of file ownership (`dir /q`) and NTFS permission entries (`icacls`) for `mini_case_log.txt`.*

---

### 4.10 Task 9: Listing Processes and Identifying Browser Processes
Active system processes were exported to `process_list.txt` using `wmic process list brief`, and running Microsoft Edge browser instances (`msedge.exe`) were filtered along with their Process IDs (PIDs) and command-line arguments.

![Figure 10: Process Listing and Browser Process Identification](screenshots/10_process_list.png)

## Mini Forensic Interpretation Questions (Task 12)

### 1. What is the difference between an absolute path and a relative path? Give one example of each from your lab.
An **absolute path** specifies the complete location of a file or folder starting from the root drive letter (`C:\`), regardless of the user's current working directory. A **relative path** specifies a file or folder location in relation to the current working directory without listing the full route from the drive root.

* **Absolute path example:** `C:\Users\Public\CIP-B101_Lab1C_[FIRSTNAME_LASTNAME]\outputs`
* **Relative path example:** `..\workspace2\evidence_note_copy.txt`

---

### 2. Why should analysts create a separate lab/evidence folder before running commands?
Creating an isolated workspace prevents accidental overwriting, corruption, or deletion of native operating system files. It also keeps all collected artifacts organized in one verifiable location and maintains proper evidence-handling hygiene without cross-contamination.

---

### 3. Which command showed your IP address and which command showed your MAC address?
* **IP Address:** `ipconfig` (as well as `ipconfig /all`) displayed the system's IPv4 address.
* **MAC Address:** `wmic nic get macaddress,description` (as well as `ipconfig /all`) displayed the physical MAC address of the network adapter.

---

### 4. Why is a batch file useful in forensic triage?
A batch file automates a predefined sequence of commands so analysts can collect volatile and non-volatile system data rapidly, consistently, and repeatably across multiple systems while minimizing manual typing errors.

---

### 5. Why must analysts document failed commands or failed pings instead of hiding them?
Accurate forensic documentation requires recording the exact state of the system as observed. A failed ping or command provides critical investigative context—such as proving that a host was network-isolated, blocked by a firewall, or offline—and preserves the integrity and credibility of the analyst's report.

---

### 6. What risk can occur if a suspicious executable is placed in a directory listed in `PATH`?
When a command is run without a full path, Windows searches directories listed in `PATH` sequentially. If a malicious executable shares the name of a legitimate utility and sits in a `PATH` directory searched before `System32`, Windows may execute the attacker's binary instead of the trusted system tool (**PATH hijacking / binary planting**), leading to unauthorized code execution or privilege escalation.

---

## Conclusion
Through this laboratory exercise, foundational Windows command-line skills essential for digital forensic triage were practiced within a controlled Windows 11 VirtualBox environment. By navigating directory structures, safely managing files, extracting system, network, and process metadata, inspecting file permissions, and building an automated batch script (`sys_info.bat`), this lab demonstrated how disciplined command-line workflows produce reliable, repeatable, and well-documented forensic evidence.
