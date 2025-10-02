# DISA STIG RHEL9 Compliance Lab Report

## Overview

**Objective:**  
Harden a RHEL9 virtual machine according to DISA STIG requirements, leveraging RMF processes and mapping findings/remediation to NIST SP 800-53 security controls.

**Environment:**  
- Host OS: Windows 10/11  
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
   - Deployed RHEL9 VM, performed system updates (`sudo dnf update -y`).

2. **Vulnerability Scan:**  
   - Ran Nessus Essentials scan; identified limitations in coverage for DISA STIG with the free version.
   - Pivoted to SCC Tool with SCAP RHEL9 Benchmark.

3. **Compliance Assessment:**  
   - Imported RHEL9 SCAP Benchmark into SCC tool.
   - Downloaded UNIX remote plugin for SCC tool to allow ssh into vm.
   - Configured and tested ssh credential connectivity within the tool. 
   - Ran intitial scan and exported `rhel9_scan_results.xml` into STIG Viewer with prebuilt RHEL9 checklist.

4. **Findings Review:**  
   - Initial compliance: 35% (per STIG Viewer).
   - Prioritized remediation of Category 1 (CAT 1) findings.

5. **Manual Remediation:**  
   - For each finding:
     - Referenced the STIG ID, associated NIST SP 800-53 control, and vulnerability description.
     - Executed appropriate Linux commands or edited configs to bring the system into compliance.
     - Re-ran SCAP/SCC scan to validate remediation.

---

### Manual Remediation Documentation


| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status | Notes |
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|-------|
| V-257784 | SV-257784r1044832_rule | RHEL-09-211045 | CAT I | AC-6 | Ctrl-Alt-Delete burst key sequence must be disabled |                   |                     | Fixed |       |

| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status | Notes |
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|-------|
| V-257785 | SV-257785r1044833_rule | RHEL-09-211050 | CAT I | AC-6 | Ctrl-Alt-Delete key sequence must be disabled |                   |                     | Fixed |       |

| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status | Notes |
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|-------|
| V-257789 | SV-257789r1117265_rule | RHEL-09-212020 | CAT I | AC-3 | RHEL 9 must require a unique superusers name upon booting into single-user and maintenance modes |                   |                     | Fixed |       |

| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status | Notes |
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|-------|
| V-257821 | SV-257821r1015077_rule | RHEL-09-214020 | CAT I | CM-14, CM-5 | RHEL 9 must check the GPG signature of locally installed software packages before installation |                   |                     | Fixed |       |

| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status | Notes |
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|-------|
| V-257879 | SV-257879r1045454_rule | RHEL-09-231190 | CAT I | SC-28 | RHEL 9 local disk partitions must implement cryptographic mechanisms to prevent unauthorized disclosure or modification of all information that requires at rest protection |                   |                     | Fixed |       |

| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status | Notes |
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|-------|
| V-257984 | SV-257984r1045026_rule | RHEL-09-255040 | CAT I | IA-2 | RHEL 9 SSHD must not allow blank passwords |                   |                     | Fixed |       |

| Vuln ID | Rule ID | STIG ID | Severity | SP 800-53 Control | Description | Remediation Steps | Commands/Edits Used | Status | Notes |
|---------|---------|---------|----------|-------------------|-------------|-------------------|---------------------|--------|-------|
| V-2580108 | SV-258018r1045090_rule | RHEL-09-271040 | CAT I | CM-6 | RHEL 9 must not allow unattended or automatic logon via the graphical user interface |                   |                     | Fixed |       |


---

## Conclusion

This lab exercise demonstrated the end-to-end process of applying the RMF, mapping vulnerabilities to NIST controls, leveraging industry-standard tools, and documenting compliance activities. This approach ensures systems are hardened against threats and meet federal compliance standards.
