# Glance Configuration Templates

A collection of YAML configuration templates for [Glance](https://github.com/glanceapp/glance) - a self-hosted dashboard that puts all your feeds in one place.

## 📋 Table of Contents

- [About](#about)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)
- [Resources](#resources)

## About

This repository contains pre-configured templates and setup scripts for deploying Glance on Proxmox. Glance is a minimalist dashboard that aggregates your favorite feeds, services, and information into a single, customizable interface.

## Prerequisites

- Proxmox VE environment
- Basic knowledge of Linux command line
- Git installed (will be installed during setup if not present)

## Installation

### 1. Install Glance on Proxmox

Use the community script to install Glance:

```bash
# Visit https://community-scripts.github.io/ProxmoxVE/scripts?id=glance
# Follow the installation instructions for your Proxmox version
```

### 2. Clone This Repository

Once Glance is installed, navigate to the installation directory and set up the configuration:

```bash
cd /opt/glance
rm glance.yml
apt install git -y
git clone https://github.com/yannisduvignau/glance-conf.git
cd glance-conf
```

### 3. Set Up FTP Access (Optional)

To access and edit files from your computer via FTP:

```bash
git clone https://github.com/yannisduvignau/setup_ftp_lxc.git
cd setup_ftp_lxc
chmod +x setup_ftp_complete.sh
./setup_ftp_complete.sh
```

Configure firewall rules:

```bash
ufw allow 8080/tcp
```

Set up FTPS (secure FTP):

```bash
chmod +x setup_ftps.sh
./setup_ftps.sh
cd ..
rm -rf setup_ftp_lxc
```

## Configuration

### 1. Set Up Environment Variables

Create your production environment file from the template:

```bash
cp template.env prod.env
nano prod.env
```

Add your API keys, credentials, and other sensitive information to `prod.env`.

### 2. Update Glance Service

Configure the systemd service to use your environment file:

```bash
nano /etc/systemd/system/glance.service
```

Add or update the following line in the `[Service]` section:

```ini
[Service]
EnvironmentFile=/opt/glance/glance-conf/prod.env
```

### 3. Restart Glance

Apply the changes by reloading the daemon and restarting the service:

```bash
systemctl daemon-reload
systemctl restart glance
```

### 4. Access Your Dashboard

Open your browser and navigate to:

```
http://your-proxmox-ip:8080
```

## Troubleshooting

If you encounter issues, check the service status:

```bash
systemctl status glance
```

View logs for more detailed information:

```bash
journalctl -u glance -f
```

Common issues:
- **Service won't start**: Check that the `prod.env` file exists and has correct permissions
- **Widgets not loading**: Verify API keys in your `prod.env` file
- **Port conflicts**: Ensure port 8080 is not in use by another service

## Resources

### Official Documentation
- [Glance GitHub Repository](https://github.com/glanceapp/glance)
- [Configuration Documentation](https://github.com/glanceapp/glance/blob/main/docs/configuration.md)
- [Community Widgets](https://github.com/glanceapp/community-widgets)

### Tutorials & Inspiration
- [Video Tutorial: Setup Guide](https://www.youtube.com/watch?v=9QCdPP9rujc)
- [Video: Configuration Examples](https://www.youtube.com/watch?v=yUyxJr2xboI&t=1873s)
- [Example Configuration](https://github.com/TechHutTV/homelab/blob/main/glance.yml)

### Installation Resources
- [Proxmox Community Scripts](https://community-scripts.github.io/ProxmoxVE/scripts?id=glance)
- [Widget Documentation](https://github.com/glanceapp/glance/blob/main/docs/configuration.md#weather)

## License

This configuration repository is provided as-is for personal use. Please refer to the [Glance project license](https://github.com/glanceapp/glance/blob/main/LICENSE) for the main application.

---

**Note**: Remember to keep your `prod.env` file secure and never commit it to version control as it contains sensitive information.