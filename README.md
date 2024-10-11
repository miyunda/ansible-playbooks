# Ansible Playbooks
[English](README.md) | [中文](README_zh-CN.md)

This repository contains Ansible playbooks primarily for my homelab and cloud servers.

## Roles
All roles share common tasks:
- [x] Set timezone to China Standard Time (UTC+08:00). Replace it with your preferred timezone in [set-timezone](set-timezone.yml).

### Caddy
**OS**: Debian 12 (Bookworm)

#### Usage
1. Edit the inventory file and the [web servers playbook](web-servers-playbook.yml).
2. Run the following command from the project root folder:

   ```bash
   ansible-playbook -i inventory.yml web-servers-playbook.yml
   ```

#### Tasks
- **Install Caddy**: Install and configure Caddy to reverse proxy Docker containers. Add additional containers in the template file [caddy.conf.j2](roles/caddy/templates/caddy.conf.j2) and define extra containers in the [vars file](roles/caddy/vars/main.yml).
- **Install Docker**: Set up Docker with the configuration specified in [daemon.json](roles/caddy/templates/docker.conf.j2). Modify options as needed, and update the username in the [defaults file](roles/caddy/defaults/main.yml).
- **Install Restic**: This task is for SQLite database backup and restore. Usage TBD.

### Kubernetes
**OS**:
- Debian 12 (Bookworm)
- Oracle Linux 8 UEK

#### Prerequisites
- [x] Ensure no duplicated hostnames or GUIDs.

> I started with a 3-node Debian 12 cluster but challenged myself with a 4th node running Oracle Linux 8. Debian is much easier for a Linux beginner like me. Adding Oracle Linux 8 was a pain but also fun!

#### Usage
Edit the inventory file and the [Kubernetes (on-prem) playbook](k8s-homelab-playbook.yml).
> ㊟ Running Ansible from macOS is not recommended, as this role contains tasks for `dnf/yum`, which may not be compatible with macOS. I had to setup a Linux VM to run Ansible.

   Run the following command from the project root folder:

   ```bash
   ansible-playbook -i inventory.yml k8s-homelab-playbook.yml
   ```

#### Tasks
- Ensure time synchronization on all nodes; NTP sources are defined in the [inventory file](inventory.yml).
- Disable swap.
- Configure networking parameters.
- Install containerd.
- Install Kubernetes components.
- Create a Kubernetes cluster and join nodes (TBD).
- Set firewalld to allow k8s traffic. (OL8 only)