# Ansible Practice Lab — Rocky Linux (ARM / macOS)

A small lab that lets you spend time on playbooks, not setup.

When I first started practicing Ansible, half my time went into fixing VMs, SSH keys, and network quirks instead of actually writing playbooks. It felt like I was learning lab plumbing instead of automation.

This lightweight Rocky Linux setup for macOS (Apple Silicon) solves that problem.  
Just spin up, practice, verify, destroy, and repeat.

---

## Overview

This lab consists of:

- One control node (for Ansible)
- Five managed nodes
- Automated `/etc/hosts` configuration for internal name resolution
- A preconfigured `student` user with passwordless sudo
- Support for VMware Fusion on ARM64

---

## Prerequisites

| Tool | Minimum version | Installation |
|------|------------------|---------------|
| macOS | 12 (Monterey) or later | – |
| Vagrant | 2.3.7 or later | `brew install --cask vagrant` |
| VMware Fusion | 13 or later | Download from VMware |
| Vagrant VMware Utility | required | See [Vagrant VMware provider docs](https://developer.hashicorp.com/vagrant/docs/providers/vmware) |
| (optional) Ansible | latest | `brew install ansible` |

---

## Usage

```bash
# Clone the repository
git clone https://github.com/<your-username>/ansible-lab-rocky-linux.git
cd ansible-lab-rocky-linux

# Start the lab
vagrant up

# Connect to the control node
vagrant ssh control

# Rebuild from scratch
vagrant destroy -f
vagrant up
