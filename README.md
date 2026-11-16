<p align="center">
  <img src="assets/Linux.png" alt="Linux" width="150">
</p>

<h1 align="center">OXA // Linux Infrastructure & Security</h1>

<p align="center">
  <code>root@oxa-linux01:~# ./build --secure --automate --document</code>
  <br>
  <strong>SYSTEM ADMINISTRATION · HOST HARDENING · AUTOMATION</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/OS-Fedora-294172?style=for-the-badge&logo=fedora&logoColor=white" alt="Fedora">
  <img src="https://img.shields.io/badge/Shell-Bash-4EAA25?style=for-the-badge&logo=gnubash&logoColor=white" alt="Bash">
  <img src="https://img.shields.io/badge/Init-systemd-FCC624?style=for-the-badge&logo=linux&logoColor=black" alt="systemd">
  <img src="https://img.shields.io/badge/SELinux-Enforcing-CC0000?style=for-the-badge&logo=redhat&logoColor=white" alt="SELinux">
  <img src="https://img.shields.io/badge/VMware-Workstation-607078?style=for-the-badge&logo=vmware&logoColor=white" alt="VMware">
  <img src="https://img.shields.io/badge/Status-In%20Progress-orange?style=for-the-badge" alt="Status">
</p>

---

```text
        .--.
       |o_o |        OXA-LNX-01
       |:_/ |        "Everything is a file. Everything is auditable."
      //   \ \
     (|     | )
    /'\_   _/`\
    \___)=(___/
```

```console
[labadmin@oxa-linux01 ~]$ neofetch --oxa
 ───────────────────────────────────────────
  OS ........ Fedora Linux
  Host ...... VMware Workstation (Windows 10)
  Hostname .. oxa-linux01
  Network ... NAT
  Shell ..... Bash
  Init ...... systemd
  Firewall .. firewalld
  MAC ....... SELinux (enforcing)
  Audit ..... auditd
  Data ...... /srv/oxa
  Project ... OXA-LNX-01  [ IN PROGRESS ]
 ───────────────────────────────────────────
```

## `man oxa` — Overview

**OXA Linux Infrastructure** is a hands-on Linux administration and security lab built on Fedora Linux inside VMware Workstation.

The project simulates the preparation and administration of a Linux server, with an emphasis on **access management, system hardening, security monitoring, and operational automation**.

The goal is to move beyond basic commands and build a structured administration workflow based on practical configuration, testing, and technical documentation.

| Field        | Value                         |
| :----------- | :---------------------------- |
| Project ID   | `OXA-LNX-01`                  |
| Server       | `oxa-linux01`                 |
| Environment  | Fedora Linux Virtual Machine  |
| Status       | In Progress                   |

---

## `tree /` — Infrastructure Design

A central Fedora server with separate identity, security, and automation components.

```text
                    WINDOWS 10 HOST
                           │
                    VMware Workstation
                           │
                       NAT NETWORK
                           │
                    FEDORA SERVER
                      oxa-linux01
                           │
          ┌────────────────┼────────────────┐
          │                │                │
       IDENTITY          SECURITY        AUTOMATION
          │                │                │
       labadmin         firewalld        Bash scripts
       analyst          SELinux          systemd
       itops             auditd          backup timer
       employees         SSH
          │
      /srv/oxa
          │
     DATA ACCESS CONTROL
```

## `cat /etc/oxa/environment`

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
| Firewall                 | firewalld            |
| Mandatory Access Control | SELinux              |
| Security Auditing        | auditd               |
| Automation               | Bash & systemd       |

---

## `./roadmap.sh` — Implementation Phases

Seven phases, each one configured, tested, and documented.

| Phase | Implementation               | Main Technologies          | Key Commands                          |
| :---: | :--------------------------- | :------------------------- | :------------------------------------ |
|  `00` | Virtual Machine Preparation  | VMware, NAT                | snapshot                              |
|  `01` | Fedora Initialization        | DNF, NetworkManager        | `dnf` `hostnamectl` `nmcli` `ip`      |
|  `02` | Identity & Access Management | Users, Groups, Permissions | `useradd` `usermod` `chmod` `chown`   |
|  `03` | Remote Access Hardening      | OpenSSH                    | `ssh-keygen` `sshd_config`            |
|  `04` | Host Security                | firewalld, SELinux         | `firewall-cmd` `getenforce` `restorecon` |
|  `05` | Security Monitoring          | auditd, journalctl         | `ausearch` `aureport` `journalctl`    |
|  `06` | Backup Automation            | Bash, systemd              | `systemctl` `systemd-analyze`         |
|  `07` | Final Validation             | Linux diagnostic tools     | `ss` `systemctl` `ls -Z`              |

<details>
<summary><code>[00]</code> Virtual Machine Preparation</summary>

Prepare the virtual environment, allocate system resources, configure networking, and create a recovery snapshot.
</details>

<details>
<summary><code>[01]</code> Fedora Initialization</summary>

Update the operating system, configure the server hostname, inspect network interfaces, and validate routing and DNS.
</details>

<details>
<summary><code>[02]</code> Identity & Access Management</summary>

Create administrative and standard accounts, organize users into groups, and implement controlled access to `/srv/oxa`.
</details>

<details>
<summary><code>[03]</code> SSH Hardening</summary>

Configure the OpenSSH service, implement key-based authentication, and restrict remote administrative access.
</details>

<details>
<summary><code>[04]</code> Host Security</summary>

Configure firewalld rules and apply SELinux policies while maintaining an enforcing security posture.
</details>

<details>
<summary><code>[05]</code> Security Monitoring</summary>

Deploy auditd, inspect authentication events, and analyze system logs to improve security visibility.
</details>

<details>
<summary><code>[06]</code> Backup Automation</summary>

Develop a Bash backup script and schedule automated execution using systemd service and timer units.
</details>

<details>
<summary><code>[07]</code> Final Validation</summary>

Review system configuration, verify services, inspect permissions, and test the implemented security controls.
</details>

---

## `sudo hardening --layers` — Security Architecture

Multiple layers of host security, no single point of trust.

| Layer                    | Implementation          | Purpose                            |
| :----------------------- | :---------------------- | :--------------------------------- |
| Identity                 | Local accounts & groups | Account separation                 |
| Authorization            | Linux permissions       | Resource access control            |
| Remote Access            | OpenSSH                 | Secure administration              |
| Network Protection       | firewalld               | Traffic filtering                  |
| Mandatory Access Control | SELinux                 | Confinement and policy enforcement |
| Auditing                 | auditd                  | Security event records             |
| Automation               | systemd                 | Scheduled administration           |

### Security Principles

```text
[*] Least Privilege
[*] Defense in Depth
[*] Secure Remote Administration
[*] Account Separation
[*] Controlled Resource Access
[*] Security Event Auditing
[*] Repeatable Administration
```

---

## `systemctl list-timers` — Automation

The automation phase removes repetitive administrative work.

```text
   SOURCE DATA
        │
        ▼
 BASH BACKUP SCRIPT        scripts/oxa-backup.sh
        │
        ▼
 BACKUP DESTINATION        /var/backups/oxa
        │
        ▼
 SYSTEMD SERVICE           oxa-backup.service
        │
        ▼
 SYSTEMD TIMER             oxa-backup.timer
        │
        ▼
 SCHEDULED EXECUTION
```

```text
scripts/
└── oxa-backup.sh

systemd/
├── oxa-backup.service
└── oxa-backup.timer
```

---

## `ls -la` — Repository Structure

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
│   ├── phase-00/ … phase-07/
│
└── .gitignore
```

## `journalctl -u evidence` — Technical Evidence

Every phase is backed by screenshots and configuration proof.

| Evidence              | Expected Content                        |
| :-------------------- | :-------------------------------------- |
| System Initialization | Hostname, OS and network configuration  |
| Identity Management   | Users, groups and permissions           |
| SSH Security          | Service status and authentication tests |
| Firewall              | Active zone and authorized services     |
| SELinux               | Enforcement mode and file contexts      |
| Audit System          | auditd status and event records         |
| Backup Automation     | Script execution and generated archives |
| Final Validation      | Service and security verification       |

> **Rule #1:** all evidence must be real test output. Credentials, private keys, and authentication data are **never** committed. Check `.gitignore` before every `git push`.

---

## `systemctl status oxa` — Project Status

```text
[  OK   ]  Fedora Installation
[  OK   ]  VMware Environment
[ PEND  ]  Network Connectivity
[ PEND  ]  Identity Management
[ PEND  ]  SSH Hardening
[ PEND  ]  firewalld & SELinux
[ PEND  ]  auditd
[ PEND  ]  Backup Automation
[ WAIT  ]  Final Validation
```

Status is updated as each phase is implemented and tested.

## `less documentation/` — Docs

The complete practical guide includes:

- Step-by-step implementation instructions
- Required Linux commands
- Configuration procedures
- Validation checkpoints
- Screenshot instructions
- Troubleshooting guidance

**Guide:** [`documentation/OXA_Linux_Lab_01_Complete_Guide.pdf`](documentation/OXA_Linux_Lab_01_Complete_Guide.pdf)

---

<p align="center">
  <img src="assets/joinus.png" alt="Join Us" width="420">
</p>

<p align="center">
  <strong>OXA LINUX INFRASTRUCTURE</strong>
  <br>
  <code>BUILD · SECURE · AUTOMATE · DOCUMENT</code>
  <br>
  <sub>[labadmin@oxa-linux01 ~]$ <code>exit</code></sub>
</p>
