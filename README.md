# Odoo Installation Guide for Ubuntu 24.04

This repository contains automated installation scripts for deploying Odoo on Ubuntu 24.04 LTS. It includes dedicated installation files for both Odoo 18 and Odoo 19, allowing you to set up a clean and production-ready Odoo environment with minimal manual configuration.

The project currently includes the following scripts:

- `odoo18_install_script.sh` — installation script for Odoo 18
- `odoo19_install_script.sh` — installation script for Odoo 19

Each script is created as a separate standalone installer and can be used independently without modifying the other.

# Features Overview for Odoo Installation Script

This section outlines the key capabilities and highlights of the installation scripts available in this repository.

## ✅ One-Command Full Installation

Install Odoo, PostgreSQL, Python dependencies, systemd service, and configuration in a single automated workflow.

## 🌐 Optional Nginx with SSL (Let's Encrypt)

Automatic setup of Nginx reverse proxy with support for SSL certificate generation and renewal using Certbot.

## 🔐 Secure by Default

- Generates a random strong master password
- Creates a system user with restricted permissions
- Includes guidance for enabling and configuring UFW firewall
- Configured systemd service with basic hardening options

## 🧪 Python Virtual Environment Support

Installs and runs Odoo in an isolated Python environment for cleaner dependency management and better compatibility.

## 🔁 Enterprise Edition Compatible

Supports the Enterprise deployment workflow when enabled and when valid access to the official Odoo Enterprise repository is available.

## 🛠️ Customizable Parameters

All major install settings (port, system user, directory paths, domain, database user, etc.) are declared at the top of each script.

## 📦 Wkhtmltopdf Auto Installer

Installs the required `wkhtmltopdf` package for generating reports and PDFs.

## 📄 Service Management Made Easy

- Installs systemd service for Odoo
- Logs saved to `/var/log/<odoo-user>`
- Easy commands to start, stop, restart, and monitor the service

## 🧹 Clean and Minimal Footprint

Minimal dependencies, clear folder structure, and straightforward installation steps without unnecessary complexity.

## 🆙 Upgrade-Friendly Structure

The installation layout supports manual or scripted updates without requiring a full reinstall of the server environment.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration Options](#configuration-options)
- [Enterprise Edition](#enterprise-edition)
- [Post-Installation](#post-installation)
- [Troubleshooting](#troubleshooting)
- [Upgrading](#upgrading)
- [Security Recommendations](#security-recommendations)

## Prerequisites

- A server with Ubuntu 24.04 LTS installed
- Sudo/root access
- A domain name (if you plan to use Nginx with SSL)
- At least 4GB of RAM (8GB recommended for production)
- At least 20GB of free disk space

## Installation

### Step 1: Download the Installation Script

```bash
# Odoo 18
wget https://raw.githubusercontent.com/fadilsnt/odoo-installer/main/odoo18_install_script.sh
# or
curl -O https://raw.githubusercontent.com/fadilsnt/odoo-installer/main/odoo18_install_script.sh
```

```bash
# Odoo 19
wget https://raw.githubusercontent.com/fadilsnt/odoo-installer/main/odoo19_install_script.sh
# or
curl -O https://raw.githubusercontent.com/fadilsnt/odoo-installer/main/odoo19_install_script.sh
```

### Step 2: Review and Customize (Optional)

Open the script to customize installation parameters:

```bash
nano odoo18_install_script.sh
```

or

```bash
nano odoo19_install_script.sh
```

### Step 3: Make the Script Executable

```bash
chmod +x odoo18_install_script.sh
```

or

```bash
chmod +x odoo19_install_script.sh
```

### Step 4: Run the Installation Script

```bash
sudo ./odoo18_install_script.sh
```

or

```bash
sudo ./odoo19_install_script.sh
```

The installation will take a few minutes. When completed, you'll see a summary of your installation details.

## Configuration Options

You can customize the installation by modifying the variables at the top of the script:

| Parameter                  | Description                      | Default Value                 |
| -------------------------- | -------------------------------- | ----------------------------- |
| `OE_USER`                  | System user for Odoo             | `odoo18` / `odoo19`           |
| `OE_HOME`                  | Installation directory           | `/opt/odoo18` / `/opt/odoo19` |
| `OE_PORT`                  | Main HTTP port                   | `8069`                        |
| `OE_VERSION`               | Odoo version                     | `18.0` / `19.0`               |
| `INSTALL_WKHTMLTOPDF`      | Install wkhtmltopdf              | `True`                        |
| `GENERATE_RANDOM_PASSWORD` | Generate random admin password   | `True`                        |
| `INSTALL_NGINX`            | Install and configure Nginx      | `True`                        |
| `WEBSITE_NAME`             | Domain name for Nginx            | `_`                           |
| `ENABLE_SSL`               | Configure SSL with Let's Encrypt | `False`                       |
| `IS_ENTERPRISE`            | Install Enterprise Edition       | `False`                       |
| `CREATE_VIRTUAL_ENV`       | Use Python virtual environment   | `True`                        |

## Enterprise Edition

⚠️ **IMPORTANT**: To use the Enterprise Edition:

1. Set `IS_ENTERPRISE="True"` in the script.
2. You **MUST** have valid access to the official Odoo Enterprise GitHub repository (https://github.com/odoo/enterprise).
3. The script will provide instructions for manually cloning the enterprise repository.

Enterprise installation requires:

- A valid Odoo Enterprise subscription
- GitHub access credentials provided by Odoo SA
- Acceptance of the Odoo Enterprise License Agreement

Without valid access credentials, the Enterprise installation will fail.

## Post-Installation

### Accessing Odoo

After installation:

1. Open your web browser and navigate to:
   - `http://your_server_ip:8069` (if not using Nginx)
   - `http://your_domain` (if using Nginx)

2. Create your first database:
   - You'll need the admin password shown at the end of the installation.
   - This password is also stored in the generated config file as `admin_passwd`.

### Managing the Odoo Service

For Odoo 18:

```bash
# Start Odoo
sudo systemctl start odoo18-server

# Stop Odoo
sudo systemctl stop odoo18-server

# Restart Odoo
sudo systemctl restart odoo18-server

# Check Status
sudo systemctl status odoo18-server

# View Logs
sudo tail -f /var/log/odoo18/odoo18-server.log
```

For Odoo 19:

```bash
# Start Odoo
sudo systemctl start odoo19-server

# Stop Odoo
sudo systemctl stop odoo19-server

# Restart Odoo
sudo systemctl restart odoo19-server

# Check Status
sudo systemctl status odoo19-server

# View Logs
sudo tail -f /var/log/odoo19/odoo19-server.log
```

## Troubleshooting

### Common Issues

1. **Port already in use**
   - Change the `OE_PORT` in the configuration file
   - Restart the service

2. **Database connection problems**
   - Check PostgreSQL is running: `sudo systemctl status postgresql`
   - Verify database user permissions

3. **Permission issues**
   - Run: `sudo chown -R odoo18:odoo18 /opt/odoo18` for Odoo 18
   - Run: `sudo chown -R odoo19:odoo19 /opt/odoo19` for Odoo 19
   - Check log file permissions for the corresponding user

4. **Nginx configuration issues**
   - Check syntax: `sudo nginx -t`
   - Check logs: `sudo tail -f /var/log/nginx/error.log`

## Upgrading

⚠️ **ALWAYS backup your database before upgrading!**

For minor version upgrades:

1. Stop the Odoo service:

   ```bash
   sudo systemctl stop odoo18-server
   ```

   or

   ```bash
   sudo systemctl stop odoo19-server
   ```

2. Update the Odoo source code:

   ```bash
   sudo su - odoo18 -c "cd /opt/odoo18/odoo && git pull"
   ```

   or

   ```bash
   sudo su - odoo19 -c "cd /opt/odoo19/odoo && git pull"
   ```

3. Update dependencies:

   ```bash
   sudo su - odoo18 -c "/opt/odoo18/venv/bin/pip install -r /opt/odoo18/odoo/requirements.txt"
   ```

   or

   ```bash
   sudo su - odoo19 -c "/opt/odoo19/venv/bin/pip install -r /opt/odoo19/odoo/requirements.txt"
   ```

4. Restart the service:

   ```bash
   sudo systemctl start odoo18-server
   ```

   or

   ```bash
   sudo systemctl start odoo19-server
   ```

## Security Recommendations

1. **Change default ports**
   - Edit the Odoo config file and change `http_port`
   - Remember to update Nginx configuration if used

2. **Use strong passwords**
   - The script generates a random master password by default
   - Ensure database users have strong passwords

3. **Keep your system updated**

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

4. **Enable firewall**

   ```bash
   sudo ufw allow ssh
   sudo ufw allow http
   sudo ufw allow https
   sudo ufw enable
   ```

5. **Regular backups**
   - Set up automated database backups
   - Store backups securely offsite

---

For additional help, visit the official Odoo documentation:

- [Odoo 18 Documentation](https://www.odoo.com/documentation/18.0/)
- [Odoo 19 Documentation](https://www.odoo.com/documentation/19.0/)

You can also submit an issue on the project repository for support or improvements.
