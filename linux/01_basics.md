# Linux

## Linux in Context

### Operating System Purpose
- An Operating System (OS) acts as a management layer between hardware and software.
- It manages:
  - CPU allocation
  - Memory allocation
  - Hardware access
  - Resource coordination
  - Communication between applications and hardware

### Linux
- Linux is an Operating System.
- The core of Linux is the **Linux Kernel**.
- Linux is:
  - Free
  - Open Source
  - Community-driven
  - Widely used across modern computing environments

### Why Linux Is Preferred Over Windows

| Area | Linux Advantage |
|--------|--------|
| Cost | Free and open source |
| Maintenance | Lower maintenance requirements |
| Performance | Uses fewer system resources |
| Scalability | Works from embedded devices to large data centers |
| Security | Strong user privilege separation |
| Updates | Frequent and transparent security updates |
| Stability | Can run for years with minimal downtime |

### Linux Machine Structure

```text
+----------------------------------------------------+
| User Applications (Vim, Docker, Apache, etc.)     |
+----------------------------------------------------+
| Shell (Bash, Zsh, Fish, etc.)                      |
+----------------------------------------------------+
| System Libraries (glibc, libc, OpenSSL, etc.)     |
+----------------------------------------------------+
| System Utilities (ls, grep, systemctl, etc.)      |
+----------------------------------------------------+
| Linux Kernel                                       |
+----------------------------------------------------+
| Hardware                                           |
+----------------------------------------------------+
```

### Linux Kernel

#### Responsibilities

| Area | Responsibility |
|--------|--------|
| Process Management | Scheduling and multitasking |
| Memory Management | RAM allocation and deallocation |
| Device Drivers | Hardware interaction |
| File System Management | Data storage and retrieval |
| Network Management | System communication |

### Shell

#### Purpose
- Command interpreter between user and kernel.
- Converts commands into actions executed by the system.

#### Examples
- Bash
- Zsh
- Fish
- Dash
- Ksh

### Applications
- User-facing software.
- Examples:
  - Vim
  - Docker
  - Apache
  - Browsers
  - Editors
  - DevOps tools

---

## Linux Distributions

### Definition
A Linux Distribution (Distro) packages:

- Linux Kernel
- System Utilities
- Software Packages
- Package Manager
- Additional Features

Different organizations and communities customize Linux and release their own distributions.

### Popular Distributions

| Distribution | Notes |
|-------------|--------|
| Ubuntu | Beginner-friendly, strong community support |
| Debian | Stable and reliable |
| Fedora | Early access to newer technologies |
| Arch Linux | Lightweight and highly customizable |
| Kali Linux | Security and penetration testing |
| Alpine Linux | Lightweight and container-focused |
| CentOS | Discontinued, replaced by AlmaLinux and Rocky Linux |

### Examples
- Ubuntu
- Red Hat
- Kali Linux

## Package Managers

### Definition
A Package Manager automates:

- Software installation
- Updates
- Dependency handling
- Configuration
- Removal

### Why Package Managers Exist
- Software is stored in central repositories.
- Packages are downloaded from trusted sources.
- Dependencies are handled automatically.
- Software management becomes simple and consistent.

### Workflow

```text
Repository
     │
Download Package
     │
Resolve Dependencies
     │
Install
     │
Configure
```

### Repository Process

1. Package manager checks configured repositories.
2. Downloads required package.
3. Downloads dependencies.
4. Installs software.
5. Configures software.

### Package Managers by Distribution

| Distribution | Package Manager | Example |
|-------------|----------------|---------|
| Ubuntu / Debian | apt | sudo apt install nginx |
| Fedora / RHEL | dnf | sudo dnf install nginx |
| Arch Linux | pacman | sudo pacman -S nginx |
| OpenSUSE | zypper | sudo zypper install nginx |

### APT Basics

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install nginx
sudo apt remove nginx
sudo apt autoremove
sudo apt search nginx
```

### DNF Basics

```bash
sudo dnf check-update
sudo dnf update
sudo dnf install nginx
sudo dnf remove nginx
```

### Pacman Basics

```bash
sudo pacman -Syu
sudo pacman -S nginx
sudo pacman -R nginx
```

### Zypper Basics

```bash
sudo zypper refresh
sudo zypper update
sudo zypper install nginx
sudo zypper remove nginx
```

### Ubuntu Repository Example

```text
Types: deb
URIs: http://ports.ubuntu.com/ubuntu-ports/
Suites: noble noble-updates noble-backports noble-security
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

### Why Run Updates After Installation

```bash
apt install sudo
sudo apt update
sudo apt upgrade -y
```

Purpose:
- Refresh package lists.
- Fetch latest repository information.
- Install newer package versions.

### Best Practices

```bash
sudo apt update && sudo apt upgrade -y
sudo apt autoremove
```

Automatic Security Updates:

```bash
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
```

