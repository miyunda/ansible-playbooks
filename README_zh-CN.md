# Ansible Playbooks
[English](README.md) | [中文](README_zh-CN.md)

自用仓库，主要是用于我的homelab服务器，也有云服务商提供的服务器。

## Roles
这里列出所有roles共同的任务:
- [x] 将时区修改为中国标准时间（UTC+08:00）。要是您不在国内，将 [set-timezone](set-timezone.yml)替换为您的首选时区。

### Caddy
**OS**: Debian 12 (Bookworm)

#### 用法
1. 编辑inventory文件和web服务器[web servers playbook](web-servers-playbook.yml).
2. 在项目的根文件夹运行:

   ```bash
   ansible-playbook -i inventory.yml web-servers-playbook.yml
   ```

#### 任务
- **安装 Caddy**: 安装Caddy并配置Caddy来反向代理Docker容器。编辑模版文件[caddy.conf.j2](roles/caddy/templates/caddy.conf.j2)和[vars file](roles/caddy/vars/main.yml)，定义额外的容器。
- **安装 Docker**: 安装Docker并设置它的配置文件 [daemon.json](roles/caddy/templates/docker.conf.j2)。修改[defaults file](roles/caddy/defaults/main.yml)指定非root用户的用户名。
- **安装 Restic**: 安装SQLite数据库备份工具。用法下次一定。

### Kubernetes
**OS**:
- Debian 12 (Bookworm)
- Oracle Linux 8 UEK

#### 前提
- [x] 没有重复的主机名或GUID。

> 我一开始在家里搞了3台Debian 12虚机做实验。后来不知道为什么脑子又抽了，想挑战自己一把，搞了个第4节点用的OL8。好后悔，难怪别人说Linux小白还是搞Debian最友好。

#### 用法
编辑inventory文件还有[Kubernetes (on-prem) playbook](k8s-homelab-playbook.yml)。
> ㊟ 不建议从macOS运行Ansible去运行远端的`dnf/yum`，这玩意的Python框架和macOS的不一样，运行不了。我被迫搞了个Linux虚机，用来运行Ansible，Docker什么的应该也行。
   在项目的根文件夹运行:

   ```bash
   ansible-playbook -i inventory.yml k8s-homelab-playbook.yml
   ```

#### 任务
- 确保对时服务都正常工作；编辑[inventory file](inventory.yml)以修改时间服务器上游。
- 仅用磁盘交换。
- 调整一些网络参数。
- 安装containerd。
- 安装Kubernetes御三家。.
- 创建一个Kubernetes集群并加入节点，下次一定。
