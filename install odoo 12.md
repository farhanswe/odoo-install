# How to Install & Configure Odoo 12 on Ubuntu

## Getting Started
First, it is recommended to update your system with latest version.

You can do it with the following command:

```sh
sudo apt-get update -y
sudo apt-get upgrade -y
```

Once your system is updated, restart the system to apply all the changes:

Next, you will need to install some packages required by Odoo to your server.

You can install all of them by running the following command:

```sh
sudo apt install git python3-pip build-essential wget python3-dev python3-venv python3-wheel libxslt-dev libzip-dev libldap2-dev libsasl2-dev python3-setuptools node-less -y
```
Once all the packages are installed, you can proceed to the next step.

## Install and Configure PostgreSQL

Before installing PostgreSQL, you will need to create a new system user for Odoo. 

You can do it with the following command:
```sh
sudo useradd -m -d /opt/odoo12 -U -r -s /bin/bash odoo12
```

Next, install PostgreSQL with the following command:

```sh
sudo apt-get install postgresql -y
```
Once the installation is completed, 

you can check the status of PostgreSQL with the following command:

```sh
sudo systemctl status postgresql
```
Next, you will need to create a new PostgreSQL for Odoo. 
You can create it with the following command:

```sh
sudo su - postgres -c "createuser -s odoo12"
```
## Install and Configure Odoo

First, you will need to download the latest version of Odoo from the Git repository. 

You can do it with the following command:
```sh
sudo su - odoo12
cd /opt/odoo12/
git clone https://www.github.com/odoo/odoo --depth 1 --branch 12.0
```
After downloading Odoo, create a new Python virtual environment for 

the Odoo 12 installation with the following command:

```sh
cd /opt/odoo12
python3 -m venv odoo12-venv
```
Next, activate the environment with the following command:

```sh
source odoo12-venv/bin/activate
```

Next, install all the required packages with the following command:

```sh
pip3 install wheel
pip3 install -r odoo/requirements.txt
```

Next, disconnect from the environment with the following command:

```sh
deactivate
```

Next, you will need to create a new directory for the custom addons. You can do it with the following command:

```sh
mkdir /opt/odoo12/odoo-custom-addons
```

Next, exit from the Odoo13 user with the following command:

```sh
exit
```

Next, copy the sample odoo13 configuration file with the following command:

```sh
cp /opt/odoo12/odoo/debian/odoo.conf /etc/odoo12.conf
```

Next, open the /etc/odoo13.conf file with the following command:

```sh
nano /etc/odoo12.conf
```

Make the following changes:

```sh
[options]
; This is the password that allows database operations:
admin_passwd = admin@123
db_host = False
db_port = False
db_user = odoo12
db_password = False
addons_path = /opt/odoo12/odoo/addons,/opt/odoo12/odoo-custom-addons
```

Save and close the file, when you are finished.

Next, you will need to create a systemd file for Odoo to manage the Odoo service. You can do this with the following command:

```sh
sudo nano /etc/systemd/system/odoo12.service
```

Add the following lines:

```sh
[Unit]
Description=Odoo12
Requires=postgresql.service
After=network.target postgresql.service
[Service]
Type=simple
SyslogIdentifier=odoo12
PermissionsStartOnly=true
User=odoo12
Group=odoo12
ExecStart=/opt/odoo12/odoo12-venv/bin/python3 /opt/odoo12/odoo/odoo-bin -c /etc/odoo12.conf
StandardOutput=journal+console
[Install]
WantedBy=multi-user.target
```

Save and close the file. Then, reload the systemd daemon with the following command:

```sh
sudo systemctl daemon-reload
```

Next, start the Odoo service with the following command:

```sh
sudo systemctl start odoo12
```
Odoo service check with the following command:

```sh
systemctl status odoo
```

