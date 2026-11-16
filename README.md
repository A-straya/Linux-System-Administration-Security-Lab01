# OXA — Linux Infrastructure & Security

\<p align="center">   \<img src="assets/Linux.png" alt="Linux" width="520"> \</p>

\<p align="center">   \<strong>LINUX SYSTEM ADMINISTRATION · INFRASTRUCTURE SECURITY · AUTOMATION\</strong>   \<br>   Fedora Linux | VMware Workstation | Bash | systemd \</p>

\<p align="center">   \<img src="[https://img.shields.io/badge/OS-Fedora-294172?style=flat-square&logo=fedora&logoColor=white](https://img.shields.io/badge/OS-Fedora-294172?style=flat-square\&logo=fedora\&logoColor=white)">   \<img src="[https://img.shields.io/badge/Virtualization-VMware-607078?style=flat-square&logo=vmware&logoColor=white](https://img.shields.io/badge/Virtualization-VMware-607078?style=flat-square\&logo=vmware\&logoColor=white)">   \<img src="[https://img.shields.io/badge/Focus-Linux%20Security-75B900?style=flat-square](https://img.shields.io/badge/Focus-Linux%20Security-75B900?style=flat-square)">   \<img src="[https://img.shields.io/badge/Level-Intermediate-555555?style=flat-square](https://img.shields.io/badge/Level-Intermediate-555555?style=flat-square)"> \</p>

---

## Overview

**OXA Linux Infrastructure** is a hands-on Linux administration and security lab built on Fedora Linux inside VMware Workstation.

The project simulates the preparation and administration of a Linux server environment, with an emphasis on access management, system hardening, security monitoring, and operational automation.

The objective is to move beyond basic Linux commands and implement a structured administration workflow based on practical configuration, testing, and technical documentation.

> **Project ID:** OXA-LNX-01
> **Server:** `oxa-linux01`
> **Environment:** Fedora Linux Virtual Machine
> **Project Status:** In Progress

## Infrastructure Design

The environment is organized around a central Fedora server, with separate administration, security, and automation components.

```text
                    WINDOWS 10 HOST
                           |
                    VMware Workstation
                           |
                       NAT NETWORK
                           |
                    FEDORA SERVER
                      oxa-linux01
                           |
          +----------------+----------------+
          |                |                |
       IDENTITY          SECURITY        AUTOMATION
          |                |                |
       labadmin         Firewalld        Bash Scripts
       analyst          SELinux          systemd
       itops             Auditd          Backup Timer
       employees         SSH
          |
      /srv/oxa
          |
     DATA ACCESS CONTROL
```

## Environment

| Component                | Configuration        |
| :----------------------- | :------------------- |
| Operating System         | Fedora Linux         |
| Hypervisor               | VMware Workstation   |
| Host OS                  | Windows 10           |
| Hostname                 | `oxa-linux01`        |
| Administrative Account   | `labadmin`           |
| Standard Account         | `analyst`            |
| Groups                   | `itops`, `employees` |
| Data Directory           | `/srv/oxa`           |
| Firewall                 | Firewalld            |
| Mandatory Access Control | SELinux              |
| Security Auditing        | Auditd               |
| Automation               | Bash & systemd       |

## Implementation Roadmap

The project is divided into seven implementation phases.

| Phase | Implementation               | Main Technologies          |
| :---: | :--------------------------- | :------------------------- |
|   00  | Virtual Machine Preparation  | VMware, NAT                |
|   01  | Fedora Initialization        | DNF, NetworkManager        |
|   02  | Identity & Access Management | Users, Groups, Permissions |
|   03  | Remote Access Hardening      | OpenSSH                    |
|   04  | Host Security                | Firewalld, SELinux         |
|   05  | Security Monitoring          | Auditd, journalctl         |
|   06  | Backup Automation            | Bash, systemd              |
|   07  | Final Validation             | Linux Diagnostic Tools     |

### 00 — Virtual Machine Preparation

Prepare the virtual environment, allocate system resources, configure networking, and create a recovery snapshot.

### 01 — Fedora Initialization

Update the operating system, configure the server hostname, inspect network interfaces, and validate routing and DNS.

### 02 — Identity & Access Management

Create administrative and standard accounts, organize users into groups, and implement controlled access to `/srv/oxa`.

### 03 — SSH Hardening

Configure the OpenSSH service, implement key-based authentication, and restrict remote administrative access.

### 04 — Host Security

Configure Firewalld rules and apply SELinux security policies while maintaining an enforcing security posture.

### 05 — Security Monitoring

Deploy Auditd, inspect authentication events, and analyze system logs to improve security visibility.

### 06 — Backup Automation

Develop a Bash backup script and schedule automated execution using systemd service and timer units.

### 07 — Final Validation

Review system configuration, verify services, inspect permissions, and test the implemented security controls.

---

## Security Architecture

The lab applies multiple layers of host security.

| Security Layer           | Implementation          | Purpose                            |
| :----------------------- | :---------------------- | :--------------------------------- |
| Identity                 | Local Accounts & Groups | Account separation                 |
| Authorization            | Linux Permissions       | Resource access control            |
| Remote Access            | OpenSSH                 | Secure administration              |
| Network Protection       | Firewalld               | Traffic filtering                  |
| Mandatory Access Control | SELinux                 | Confinement and policy enforcement |
| Auditing                 | Auditd                  | Security event records             |
| Automation               | systemd                 | Scheduled administration           |

### Security Principles

- Least Privilege
- Defense in Depth
- Secure Remote Administration
- Account Separation
- Controlled Resource Access
- Security Event Auditing
- Repeatable Administration

## Automation

The automation phase focuses on reducing repetitive administrative operations.

**Backup workflow**

```text
       SOURCE DATA
            |
            v
     BASH BACKUP SCRIPT
            |
            v
     BACKUP DESTINATION
       /var/backups/oxa
            |
            v
      SYSTEMD SERVICE
            |
            v
       SYSTEMD TIMER
            |
            v
     SCHEDULED EXECUTION
```

Main project files:

```text
scripts/
└── oxa-backup.sh

systemd/
├── oxa-backup.service
└── oxa-backup.timer
```

## Repository Structure

```text
OXA-Linux-Infrastructure/
│
├── README.md
│
├── assets/
│   ├── Linux.png
│   └── joinus.png
│
├── documentation/
│   ├── OXA_Linux_Lab_01_Complete_Guide.pdf
│   └── architecture/
│
├── configurations/
│   ├── ssh/
│   ├── firewalld/
│   └── audit/
│
├── scripts/
│   └── oxa-backup.sh
│
├── systemd/
│   ├── oxa-backup.service
│   └── oxa-backup.timer
│
├── screenshots/
│   ├── phase-00/
│   ├── phase-01/
│   ├── phase-02/
│   ├── phase-03/
│   ├── phase-04/
│   ├── phase-05/
│   ├── phase-06/
│   └── phase-07/
│
└── .gitignore
```

## Technical Evidence

Each implementation phase is documented with screenshots and configuration evidence.

| Evidence              | Expected Content                        |
| :-------------------- | :-------------------------------------- |
| System Initialization | Hostname, OS and network configuration  |
| Identity Management   | Users, groups and permissions           |
| SSH Security          | Service status and authentication tests |
| Firewall              | Active zone and authorized services     |
| SELinux               | Enforcement mode and file contexts      |
| Audit System          | Auditd status and event records         |
| Backup Automation     | Script execution and generated archives |
| Final Validation      | Service and security verification       |

All evidence should represent actual test results. Sensitive credentials, private keys, and authentication data must never be committed to the repository.

## Project Status

| Component            | Status             |
| :------------------- | :----------------- |
| Fedora Installation  | Completed          |
| VMware Environment   | Configured         |
| Network Connectivity | Pending Validation |
| Identity Management  | Pending Validation |
| SSH Hardening        | Pending Validation |
| Firewalld & SELinux  | Pending Validation |
| Auditd               | Pending Validation |
| Backup Automation    | Pending Validation |
| Final Validation     | Pending            |

The status is updated as each phase is implemented and successfully tested.

## Documentation

The complete practical guide includes:

- Step-by-step implementation instructions
- Required Linux commands
- Configuration procedures
- Validation checkpoints
- Screenshot instructions
- Troubleshooting guidance

**Guide:** `documentation/OXA_Linux_Lab_01_Complete_Guide.pdf`

---

\<p align="center">   \<img src="assets/joinus.png" alt="Join Us" width="420"> \</p>

\<p align="center">   \<strong>OXA LINUX INFRASTRUCTURE\</strong>   \<br>   \<sub>BUILD · SECURE · AUTOMATE · DOCUMENT\</sub> \</p>
