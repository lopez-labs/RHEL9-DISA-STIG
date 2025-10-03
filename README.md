# DISA STIG RHEL9 Compliance Lab Report

## Overview

**Objective:**  
Harden a RHEL9 virtual machine according to DISA STIG requirements, leveraging RMF processes and mapping findings/remediation to NIST SP 800-53 security controls.

**Environment:**  
- Host OS: Windows 10  
- Virtualization: VMware Workstation  
- Guest OS: RHEL9  
- Vulnerability/Compliance Tools: Nessus Essentials, SCC (Security Content Compliance) Tool, SCAP RHEL9 Benchmark, STIG Viewer  
- Reference: DISA STIG for RHEL9

---

## RMF Process Alignment

- **Categorize:** System categorized as Moderate Impact per FIPS 199.
- **Select:** Applicable security controls selected from NIST SP 800-53.
- **Implement:** Security configurations implemented per STIG guidance.
- **Assess:** Vulnerability and compliance scans performed; results mapped to NIST controls.
- **Authorize:** [Lab context: Not applicable—would be submitted for AO review in production.]
- **Monitor:** Continuous monitoring simulated through iterative scan/remediation cycles.

---

## Tools & Commands Used

| Tool              | Purpose                                           | Example Command(s)                   |
|-------------------|---------------------------------------------------|--------------------------------------|
| VMware Workstation| VM management                                     | n/a                                  |
| SSH (from host)   | Remote shell access to RHEL9 VM                   | `ssh user@<RHEL9_IP>`                |
| Nessus Essentials | Initial vulnerability scan                        | Web UI                               |
| SCC Tool          | SCAP-based compliance assessment                  | Web UI                               |
| STIG Viewer       | Checklist management and results import           | n/a                                  |
| Linux CLI         | Manual remediation, config edits                  | `vi /etc/ssh/sshd_config`, etc.      |

---

## Compliance and Remediation Workflow

1. **Baseline Installation:**  
   - Downloaded RHEL9 DVD ISO, VMWare Workstation, Nessus Essentials, SCAP Content&Tool, and RHEL9 STIG along with the viewer.  
   - Booted RHEL9 inside of the VM.
   - Performed system updates (`sudo dnf update -y`).

2. **Vulnerability Scan:**  
   - Ran Nessus Essentials host discovery & <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/Baseline%20Nessus%20Scan.png">vulnerability scan</a>; identified limitations in coverage for DISA STIG with the free version.
   - Pivoted to SCC Tool with SCAP RHEL9 Benchmark.

3. **Compliance Assessment:**  
   - Opened the <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/SCC%20Initial%20Opening.png">SCC tool</a> and imported RHEL9 SCAP Benchmark.
   - Downloaded UNIX remote plugin for SCC tool to allow ssh into vm.
   - Configured and tested ssh credential connectivity within the tool. 
   - Ran intitial scan and exported results file into STIG Viewer with prebuilt RHEL9 checklist to view <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/StigViewerBaseline.png">baseline results</a>.

4. **Findings Review:**  
   - Initial compliance: <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/BaselineScapScanScore.png">35.06%</a>
   - Prioritized remediation of Category 1 (CAT 1) findings.

5. **Manual Remediation:**  
   - For each finding:
     - Referenced the STIG ID, associated NIST SP 800-53 control, and vulnerability description.
     - Executed appropriate Linux commands or edited configs to bring the system into compliance.
     - Re-ran SCAP/SCC scan to validate remediation.

---

### Manual Remediation Documentation


| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status |
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|
| V-257784 | SV-257784r1044832_rule | RHEL-09-211045 | CAT I | AC-6 | Systemd Ctrl-Alt-Delete burst key sequence must be disabled | <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/V-257784-Review.png">Verified</a> the finding <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/V-257784-Remediation.png">Edited</a> the config file | ` grep -i ctrl /etc/systemd/system.conf`| Fixed |       

| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status | 
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|
| V-257785 | SV-257785r1044833_rule | RHEL-09-211050 | CAT I | AC-6 | x86 Ctrl-Alt-Delete key sequence must be disabled | <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/V-257785-Remediation.png">Disabled&Masked</a> key sequence | `sudo systemctl disable --now ctrl-alt-del.target` `sudo systemctl mask --now ctrl-alt-del.target` | Fixed |       

| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status |
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|
| V-257789 | SV-257789r1117265_rule | RHEL-09-212020 | CAT I | AC-3 | RHEL 9 must require a unique superusers name upon booting into single-user and maintenance modes | <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/%20V-257789-Review.png">Reviewed</a> the finding, <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/V-257789-ConfigEdit.png">Edited</a> the appropriate config file, Regenerated the GRUB config& rebooted the system <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/V-257789-Remediation.png">here</a>, <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/V-257789-Verification.png">Verified</a> the fix                   | `sudo grub2-mkconfig -o /boot/grub2/grub.cfg` `sudo reboot` | Fixed |       

| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status |
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|
| V-257821 | SV-257821r1015077_rule | RHEL-09-214020 | CAT I | CM-14, CM-5 | RHEL 9 must check the GPG signature of locally installed software packages before installation | <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/V-257821-Remediation.png">Edited</a> the config file, <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/V-257821-Verification.png">Verified</a> the fix | `grep localpkg_gpgcheck /etc/dnf/dnf.conf` | Fixed |       

| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status |
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|
| V-257879 | SV-257879r1045454_rule | RHEL-09-231190 | CAT I | SC-28 | RHEL 9 local disk partitions must implement cryptographic mechanisms to prevent unauthorized disclosure or modification of all information that requires at rest protection |                   |                     | Fixed |  

| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status |
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|
| V-257984 | SV-257984r1045026_rule | RHEL-09-255040 | CAT I | IA-2 | RHEL 9 SSH must not allow blank passwords | <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/V-257984-Remediation.png">Edit</a> config file | `sudo vi /etc/ssh/sshd_config` | Fixed |   

| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status |
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|
| V-2580108 | SV-258018r1045090_rule | RHEL-09-271040 | CAT I | CM-6 | RHEL 9 must not allow unattended or automatic logon via the graphical user interface | Edit config file, <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/V-258018-Verification.png">Verify</a> the fix | `grep -i automaticlogin /etc/gdm/custom.conf` | Fixed | 


---

## Reflection

After I completed these manual remediations I reran the SCAP scan to verify the success of the fixes. The <a href="https://github.com/lopez-labs/RHEL9-DISA-STIG/blob/main/LabScreenshots/%20SCAPScanScoreAfterManualRemediation.png">SCAP scan score after remediation</a> was 36.36%, an increase of only 1.3%. Although this is a small increase overall, CAT 1 open finding vulnerabilities were reduced by 70% as is seen by this <a href="">before</a> and <a href="">after</a>.

---

## Going Forward

This lab exercise demonstrated the end-to-end process of applying the STIG/RMF, mapping vulnerabilities to NIST controls, leveraging industry-standard tools, and documenting compliance activities. This approach ensures systems are hardened against threats and meet federal compliance standards.

<a href=""></a>
