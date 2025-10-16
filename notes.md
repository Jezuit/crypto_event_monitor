# Post-Installation Configuration

---

Updated the system and all installed packages to their latest versions:
```
sudo apt update && sudo apt upgrade -y
```

Set the system's timezone to Europe/Warsaw:
```
sudo timedatectl set-timezone Europe/Warsaw
```

Added a new user account named "dogeblog" and set its password:
```
sudo adduser dogeblog
```

Installed the Nginx web server with all required dependencies:
```
sudo apt install nginx -y
```

Verified that the Nginx service was active and running:
```
sudo systemctl status nginx
```

The output confirmed the status as "active (running)".

Added the "dogeblog" user to the www-data group to grant necessary web directory permissions:
```
sudo usermod -aG www-data dogeblog
```

## Nginx Web Server Configuration

Navigated to the Nginx sites-available configuration directory:
```
cd /etc/nginx/sites-available/
```

Checked the contents of the directory:
```
ls -l
```

Created and opened a new configuration file for the site:
```
sudo nano dogeblog.local
```

Added the following server block configuration to the empty file. This configuration sets up a reverse proxy:
* listen 80; - Listen on the standard HTTP port.
* server_name dogeblog.local ...; - Respond to requests for this domain.
* location / { ... } - For all incoming requests (/)...
* proxy_pass http://127.0.0.1:8000; - forward them to the application running locally on port 8000. This is the port where the Python application will be launched.
```
server {
    listen 80;
    server_name dogeblog.local www.dogeblog.local;

    location / {
        proxy_pass [http://127.0.0.1:8000](http://127.0.0.1:8000);
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Enabled the new site configuration by creating a symbolic link from sites-available to sites-enabled:
```
sudo ln -s /etc/nginx/sites-available/dogeblog.local /etc/nginx/sites-enabled/
```

Tested the Nginx configuration for syntax errors:
```
sudo nginx -t
```

Restarted the Nginx service to apply the new configuration:
```
sudo systemctl restart nginx
```
## MariaDB Server Installation and Configuration

Install the MariaDB server package:
```
sudo apt install mariadb-server -y
```

Run the security script to secure the MariaDB installation:
```
sudo mysql_secure_installation
```

Start and enable the MariaDB service:
```
sudo systemctl start mariadb
```

Access the MariaDB command-line interface:
```
sudo mariadb
```

Create an empty database for the blog application:
```
CREATE DATABASE dogeblog_db;
```

Create a new database user named dogeblog_user with the password mbpadmin:
```
CREATE USER 'dogeblog_user'@'localhost' IDENTIFIED BY 'mbpadmin';
```

Grant the new user full privileges on the new database:
```
GRANT ALL PRIVILEGES ON dogeblog_db.* TO 'dogeblog_user'@'localhost';
```

Reload the grant tables to ensure the new permissions are applied immediately:
```
FLUSH PRIVILEGES;
```

Exit the MariaDB console:
```
EXIT;
```
## Firewall (UFW) Configuration

Allowed incoming SSH connections to ensure remote access is maintained:
```
sudo ufw allow ssh
```

Allowed incoming web traffic through Nginx using the 'Nginx Full' profile (ports 80 for HTTP and 443 for HTTPS):
```
sudo ufw allow 'Nginx Full'
```

Enabled the firewall to make the rules active:
```
sudo ufw enable
```
## Required Software Installation

* Python

Verified that Python 3 was pre-installed by checking its version:
```
python3 --version
```

The output confirmed that Python 3.12.3 was present on the system.

* Pip and venv

Installed pip (the Python package installer) and venv (the module for creating virtual environments):
```
sudo apt install python3-pip python3-venv -y
```
## Development Environment Setup

Installed Git to manage source code:
```
sudo apt install git -y
```

Created the application directory:
```
sudo mkdir -p /var/www/dogeblog
```

Set the correct ownership for the application directory:
```
sudo chown -R dogeblog:dogeblog /var/www/dogeblog
```

Switched to the dedicated application user:
```
sudo su - dogeblog
```

Navigated into the application's directory:
```
cd /var/www/dogeblog
```

Cloned the project repository into the current directory:
```
git clone [https://github.com/Jezuit/crypto_event_monitor.git](https://github.com/Jezuit/crypto_event_monitor.git) .
```

Created and activated a Python virtual environment:
```
python3 -m venv venv
```

Activated the virtual environment:
```
source venv/bin/activate
```

Installed the project's dependencies using pip:
```
pip install -r requirements.txt
```
