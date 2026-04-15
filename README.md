# RHEL Home Lab — Ansible Automation

![Ansible](https://img.shields.io/badge/Ansible-EE0000?style=flat&logo=ansible&logoColor=white)
![RHEL](https://img.shields.io/badge/RHEL-EE0000?style=flat&logo=redhat&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat&logo=github-actions&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)

A home lab project built on Red Hat Enterprise Linux (RHEL) running on Proxmox. This repository contains Ansible playbooks, roles, and a GitHub Actions CI/CD pipeline used to automate the configuration and management of multiple RHEL virtual machines.

This is an ongoing project. I am actively expanding it as I learn.

---

## Environment

| Component | Details |
|---|---|
| Hypervisor | Proxmox VE |
| Operating System | Red Hat Enterprise Linux (RHEL) |
| VM1 | Control node — Ansible, Git |
| VM2 | Managed node — Apache web server |
| VM3 | Services — Jenkins, future services |
| Network | Internal bridge (vmbr2) for VM-to-VM communication |

---

## Repository Structure

```
rhel-homelab-ansible/
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions CI/CD pipeline
├── roles/
│   ├── common/                 # Base config applied to all servers
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   ├── handlers/
│   │   │   └── main.yml
│   │   └── defaults/
│   │       └── main.yml
│   └── webserver/              # Apache web server role
│       ├── tasks/
│       │   └── main.yml
│       ├── handlers/
│       │   └── main.yml
│       ├── defaults/
│       │   └── main.yml
│       └── templates/
│           └── index.html.j2
├── group_vars/
│   └── all/
│       ├── vars.yml            # Variable definitions
│       └── vault.yml           # Encrypted secrets (Ansible Vault)
├── inventory.ini               # Host inventory
├── site.yml                    # Main playbook — applies to all hosts
├── rhel-vm2.yml                # Full configuration playbook for VM2
├── requirements.yml            # Ansible collection dependencies
└── README.md
```

---

## What This Does

### Ansible Roles

**common role** — applied to every server:
- Installs baseline packages (vim, curl, wget, net-tools)
- Ensures firewalld is running and enabled
- Sets system timezone

**webserver role** — applied to web servers:
- Installs and enables Apache (httpd)
- Opens firewall for HTTP traffic
- Deploys a dynamic index page using a Jinja2 template
- Restarts Apache automatically when config changes via handlers

### Ansible Vault
Sensitive variables like passwords and email addresses are encrypted using Ansible Vault. The vault file is safe to store in Git because it is encrypted and cannot be read without the vault password.

### GitHub Actions Pipeline
Every push to the `main` branch automatically triggers the CI/CD pipeline which:
1. Checks out the latest code
2. Installs Ansible and required collections
3. Creates SSH keys and vault password from GitHub secrets
4. Validates the playbook syntax
5. Deploys the configuration to VM2

---

## How to Use

### Prerequisites
- Ansible installed on the control node
- SSH key access to all managed hosts
- Ansible Vault password

### Clone the repo
```bash
git clone https://github.com/YOURUSERNAME/rhel-homelab-ansible.git
cd rhel-homelab-ansible
```

### Install required collections
```bash
ansible-galaxy collection install -r requirements.yml
```

### Create your vault password file
```bash
echo "yourvaultpassword" > ~/.vault_pass
chmod 600 ~/.vault_pass
```

### Run the full site playbook
```bash
ansible-playbook -i inventory.ini site.yml --vault-password-file ~/.vault_pass
```

### Run the VM2 specific playbook
```bash
ansible-playbook -i inventory.ini rhel-vm2.yml --vault-password-file ~/.vault_pass
```

### Validate a playbook without making changes
```bash
ansible-playbook -i inventory.ini rhel-vm2.yml --check --vault-password-file ~/.vault_pass
```

---

## GitHub Actions Secrets Required

To use the CI/CD pipeline you need to add these secrets to your GitHub repo under Settings → Secrets and variables → Actions:

| Secret | Description |
|---|---|
| `SSH_PRIVATE_KEY` | Private SSH key for connecting to managed hosts |
| `VAULT_PASSWORD` | Ansible Vault password for decrypting vault.yml |
| `VM2_IP` | IP address of VM2 |
| `VM3_IP` | IP address of VM3 |

---

## Labs Completed

This repo is part of a larger home lab series. The following labs have been completed:

- Lab 1 — LVM Storage Management
- Lab 2 — SELinux Troubleshooting  
- Lab 3 — Systemd & Service Management
- Lab 4 — Firewalld & Network Security
- Lab 5 — Bash Scripting for Admins
- Lab 6 — Ansible Automation
- Lab 7 — Git Basics
- Lab 8 — DNS Server with BIND
- Lab 9 — NFS File Server
- Lab 10 — Monitoring with Prometheus & Grafana
- Lab 11 — Podman Containers
- Lab 12 — Multi-Container App with Podman
- Lab 13 — Proxmox Networking & Second VM

## Expansion in Progress

- [x] Phase 1 — Ansible roles, vault, and structured automation
- [x] Phase 2 — CI/CD pipeline with GitHub Actions
- [ ] Phase 3 — OpenShift / Kubernetes
- [ ] Phase 4 — FreeIPA centralized authentication
- [ ] Phase 5 — Jellyfin media server

---

## Author

Built and maintained as a personal home lab project for learning Linux system administration and DevOps practices.
