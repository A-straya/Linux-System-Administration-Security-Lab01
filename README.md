# OXA | Linux System Administration & Security Lab

**Project:** OXA Linux Infrastructure  
**Lab ID:** OXA-LNX-01  
**Platform:** Fedora Linux  
**Virtualization:** VMware Workstation  
**Category:** Linux Administration · Infrastructure Security · Automation  
**Level:** Intermediate  
**Status:** In Progress

---

## 01. Project Overview

The OXA Linux Infrastructure Lab is a practical system administration and security project built in a Fedora virtual machine using VMware Workstation.

The objective is to simulate the configuration and administration of a Linux server in a small enterprise environment.

The lab focuses on establishing a secure and maintainable Linux environment through user and group management, access control, SSH hardening, firewall configuration, SELinux, security auditing, system monitoring, and automated backups.

Rather than focusing only on individual commands, this project follows a structured infrastructure deployment workflow:

**CONFIGURE → SECURE → AUTOMATE → VALIDATE → DOCUMENT**

<p align="center">
  <img src="assets/Linux.png" alt="Linux operating system illustration" width="65%">
</p>

## 02. Lab Objectives

- Deploy and prepare a Fedora Linux server.
- Configure the hostname and system environment.
- Create administrative and standard user accounts.
- Organize users through Linux groups.
- Apply ownership and permission policies.
- Configure and secure remote SSH access.
- Manage network access using Firewalld.
- Apply mandatory access control through SELinux.
- Configure security auditing with Auditd.
- Monitor system resources and service activity.
- Automate backups using Bash and systemd timers.
- Validate the configuration through practical tests.
- Maintain technical documentation and evidence on GitHub.

## 03. Lab Environment

| Component | Configuration |
|---|---|
| Project | OXA Linux Infrastructure |
| Operating System | Fedora Linux |
| Virtualization | VMware Workstation |
| Host System | Windows 10 |
| Server Hostname | `oxa-linux01` |
| Administrative User | `labadmin` |
| Standard User | `analyst` |
| Groups | `itops`, `employees` |
| Main Data Directory | `/srv/oxa` |
| Firewall | Firewalld |
| Access Control | SELinux |
| Auditing | Auditd |
| Automation | Bash, systemd |

**Network configuration:** VMware NAT is the intended virtual network mode. IP addressing, default gateway, DNS resolution, and external connectivity must be validated in the guest before Internet-dependent operations are considered complete.

## 04. Infrastructure Architecture

The lab follows a centralized Linux server model.

```text
                 WINDOWS 10 HOST
                       |
                VMware Workstation
                       |
                  NAT Network
                       |
                FEDORA SERVER
                 oxa-linux01
                       |
       +---------------+----------------+
       |               |                |
   USER ACCESS     SECURITY         AUTOMATION
       |               |                |
   labadmin          Firewalld       Bash Scripts
   analyst           SELinux        systemd Timers
   itops             Auditd
   employees         SSH Hardening
       |
   /srv/oxa
       |
   Controlled Data Access
```

### Main infrastructure components

| Component | Responsibility |
|---|---|
| Fedora Server | Main Linux operating environment |
| User Management | Identity and account administration |
| Linux Groups | Role-based access organization |
| File Permissions | Resource access control |
| SSH | Secure remote administration |
| Firewalld | Network traffic filtering |
| SELinux | Mandatory access control |
| Auditd | Security event auditing |
| systemd | Service and scheduled task management |
| Bash | Administrative automation |

## 05. Implementation Phases

### Phase 00 — Virtual Machine Preparation

Prepare the VMware environment before configuring the server.

- Configure the virtual machine resources.
- Verify the virtual network adapter.
- Select NAT networking.
- Check that the VM is connected.
- Create a VMware snapshot before major configuration changes.

**Expected result:** A Fedora virtual machine ready for administration.

### Phase 01 — Fedora Server Initialization

Prepare the operating system and establish the server identity.

Main operations:
- Update the Fedora system.
- Configure the hostname.
- Inspect network interfaces.
- Verify IP addressing and routing.
- Check DNS resolution.
- Confirm the status of NetworkManager.

```bash
hostnamectl
sudo hostnamectl set-hostname oxa-linux01

sudo dnf update -y

ip -br address
ip route
nmcli device status

systemctl status NetworkManager
```

**Validation:**

```bash
hostnamectl
ip -br address
ip route
```

### Phase 02 — User and Group Administration

Implement a basic identity and access organization model.

| Account | Purpose |
|---|---|
| `labadmin` | Administrative operations |
| `analyst` | Standard user account |

| Group | Purpose |
|---|---|
| `itops` | IT operations |
| `employees` | Standard workforce |

Main operations:
- Create local user accounts.
- Create administrative and organizational groups.
- Assign users to appropriate groups.
- Configure ownership of `/srv/oxa`.
- Apply restrictive permissions.
- Verify effective access.

```bash
sudo groupadd itops
sudo groupadd employees

sudo useradd -m analyst
sudo usermod -aG employees analyst

sudo mkdir -p /srv/oxa

sudo chown -R root:itops /srv/oxa
sudo chmod 2770 /srv/oxa

id analyst
getent group itops
getent group employees
ls -ld /srv/oxa
```

The setgid bit on the directory helps new files inherit the directory's group.

### Phase 03 — SSH Configuration and Hardening

Configure secure remote administration.

Security objectives:
- Install and enable the SSH service.
- Verify SSH connectivity.
- Configure key-based authentication.
- Restrict administrative access.
- Harden the SSH daemon configuration.
- Verify access before disabling password authentication.

```bash
sudo dnf install openssh-server -y

sudo systemctl enable --now sshd

sudo systemctl status sshd

sudo sshd -t
```

Inspect the SSH configuration:

```bash
sudo nano /etc/ssh/sshd_config
```

**Important:** Do not disable password authentication until key-based access has been successfully tested in a separate session. Keep an existing administrative session open during the change.

### Phase 04 — Firewall and SELinux

Protect the server using network filtering and mandatory access control.

**Firewalld**

```bash
sudo systemctl enable --now firewalld

sudo firewall-cmd --state
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --list-all
```

Example service authorization:

```bash
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload

sudo firewall-cmd --list-services
```

Use the active firewall zone when applying permanent rules.

**SELinux**

```bash
getenforce
sestatus

ls -ldZ /srv/oxa

sudo restorecon -Rv /srv/oxa
```

SELinux should remain enabled and enforcing. Access problems should be investigated through permissions, contexts, and audit logs rather than solved by disabling SELinux.

### Phase 05 — Security Auditing and Monitoring

Introduce system-level security visibility.

The lab uses Auditd to record selected security-related events.

```bash
sudo dnf install audit -y

sudo systemctl enable --now auditd

sudo systemctl status auditd

sudo auditctl -s
sudo ausearch -m USER_LOGIN
sudo journalctl -p warning -n 30
```

**Validation:** Confirm Auditd is running and that configured events can be inspected.

### Phase 06 — Backup Automation

Develop a Bash-based backup workflow and schedule it through systemd.

Main operations:
- Create a backup destination.
- Write a Bash backup script.
- Apply executable permissions.
- Test the script manually.
- Create a systemd service.
- Create a systemd timer.
- Verify scheduled execution.

```bash
sudo mkdir -p /var/backups/oxa

sudo chmod +x /usr/local/sbin/oxa-backup.sh

sudo systemctl daemon-reload
sudo systemctl enable --now oxa-backup.timer

systemctl list-timers --all
systemctl status oxa-backup.timer
```

The backup script, service unit, and timer unit are documented in the project files.

### Phase 07 — Final Validation

Perform a final review of the configured infrastructure.

**System identity**

```bash
hostnamectl
```

**Users and groups**

```bash
id labadmin
id analyst

getent group itops
getent group employees
```

**File permissions**

```bash
ls -ld /srv/oxa
```

**SSH**

```bash
sudo sshd -t
systemctl status sshd
```

**Firewall**

```bash
sudo firewall-cmd --state
sudo firewall-cmd --list-all
```

**SELinux**

```bash
getenforce
sestatus
```

**Auditing**

```bash
systemctl status auditd
sudo auditctl -s
```

**Automation**

```bash
systemctl status oxa-backup.timer
systemctl list-timers --all
```

**Network connectivity**

```bash
ip -br address
ip route
ping -c 4 8.8.8.8
ping -c 4 google.com
```

IP connectivity and DNS resolution are separate checks and must be recorded independently.

## 06. Screenshot Evidence

Screenshots are part of the technical documentation and should demonstrate actual command execution and configuration results.

| Phase | Evidence |
|---|---|
| 00 | VMware virtual machine configuration |
| 01 | Fedora hostname and system information |
| 01 | Network interfaces and routing |
| 02 | User and group configuration |
| 02 | Directory ownership and permissions |
| 03 | SSH service and configuration validation |
| 03 | Successful SSH authentication test |
| 04 | Firewalld status and authorized services |
| 04 | SELinux status and security contexts |
| 05 | Auditd service and audit results |
| 06 | Backup script and generated backup |
| 06 | systemd timer status |
| 07 | Final system validation |

Use the numbered screenshot filenames from the accompanying lab guide to keep the evidence consistent.

All screenshots should show readable terminal output, the relevant command, and its result. Avoid capturing passwords, private SSH keys, or other sensitive information.

## 07. Repository Structure

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
├── scripts/
│   └── oxa-backup.sh
│
├── systemd/
│   ├── oxa-backup.service
│   └── oxa-backup.timer
│
├── configurations/
│   ├── ssh/
│   ├── firewalld/
│   └── audit/
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

Do not upload private SSH keys, passwords, authentication tokens, private user data, or unredacted sensitive configuration files.

## 08. Troubleshooting

| Problem | Investigation |
|---|---|
| No Internet access | Check VMware NAT, IP address, gateway, and DNS |
| DNS resolution failure | Inspect `/etc/resolv.conf` and NetworkManager DNS settings |
| SSH connection refused | Check `sshd`, firewall rules, and listening ports |
| Permission denied | Inspect ownership, groups, and mode bits |
| SELinux access denial | Inspect security contexts and audit records |
| Firewall service not available | Verify Firewalld status and active zone |
| Audit events missing | Inspect Auditd status and active rules |
| Backup timer not running | Inspect systemd service and timer logs |

Useful diagnostic commands:

```bash
sudo journalctl -xe
sudo journalctl -u NetworkManager -n 50
sudo journalctl -u sshd -n 50
sudo journalctl -u firewalld -n 50
sudo ausearch -m AVC
```

Troubleshooting should follow a controlled process: identify the failing component, inspect its status and logs, correct the configuration, and repeat the validation test.

## 09. Security Principles Applied

- **Least Privilege:** Users receive only the permissions required for their roles.
- **Defense in Depth:** Firewall filtering, SELinux, SSH hardening, and auditing provide complementary security controls.
- **Secure Remote Administration:** SSH access is configured with authentication hardening.
- **Account Separation:** Administrative and standard accounts are separated.
- **Access Control:** File ownership, group membership, and permissions regulate access to resources.
- **Monitoring and Accountability:** Auditd and system logs provide visibility into system activity.
- **Automation:** Repeatable administrative operations reduce manual configuration errors.
- **Verification:** Every major configuration phase includes validation commands.

## 10. Skills Demonstrated

**Linux System Administration**
- Fedora administration
- User and group management
- Linux filesystem permissions
- systemd service management
- Package management with DNF

**Infrastructure Security**
- SSH hardening
- Firewalld configuration
- SELinux security contexts
- Auditd configuration
- System log analysis

**Automation**
- Bash scripting
- Automated backups
- systemd timers
- Administrative task scheduling

**Troubleshooting**
- Network diagnostics
- Service troubleshooting
- Permission analysis
- Security event investigation

## 11. Project Status

| Component | Status |
|---|---|
| Fedora installation | Completed |
| VMware configuration | Configured |
| Network connectivity | Pending validation |
| User and group management | To be validated |
| SSH hardening | To be validated |
| Firewalld and SELinux | To be validated |
| Auditd | To be validated |
| Backup automation | To be validated |
| Final validation | Pending |

The status table should be updated only after each phase has been executed and its results verified.

## 12. Conclusion

The OXA Linux Infrastructure Lab establishes a practical foundation in Linux system administration and infrastructure security.

By combining identity management, access control, secure remote administration, firewall configuration, mandatory access control, auditing, and backup automation, the project demonstrates how individual Linux administration tasks contribute to a more secure and manageable server environment.

This lab is part of a continuous technical learning path focused on Linux infrastructure, networking, system administration, and cybersecurity.

---

<p align="center">
  <img src="assets/joinus.png" alt="Join us" width="75%">
</p>

<p align="center">
  <strong>OXA LINUX INFRASTRUCTURE</strong><br>
  Linux Administration | Infrastructure Security | Automation
</p>

<p align="center">
  <sub>Learn. Configure. Secure. Automate.</sub>
</p>
