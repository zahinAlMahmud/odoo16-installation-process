###  Odoo  Installion
****
## Step 1: Update Package Manager

****
```
sudo apt-get update  

sudo apt-get upgrade -y 
```
****
## Step 2: Create an Odoo User

****
```
sudo adduser -system -home=/opt/odoo -group odoo
```
****
## Step 3: Step 3: Install PostgreSQL and Create an Odoo User for PostgreSQL

****
```
sudo apt install postgresql -y

sudo su – postgres -c "createuser -s odoo" 2> /dev/null || true

sudo chmod 700 -R /var/lib/postgresql/16/main/

sudo systemctl restart postgresql

```
****
## Step 4: Installation of Python and Python PIP Dependencies

****
```
sudo apt-get install git python3 python3-pip build-essential wget python3-dev python3-venv python3-wheel libxslt-dev libzip-dev libldap2-dev libsasl2-dev python3-setuptools node-less libjpeg-dev gdebi -y

sudo apt-get install libpq-dev python3-dev libxml2-dev libxslt1-dev libldap2-dev libsasl2-dev libffi-dev python3-psutil python3-polib python3-dateutil python3-decorator python3-lxml python3-reportlab python3-pil python3-passlib python3-werkzeug python3-psycopg2 python3-pypdf2 python3-gevent -y
```
****

## Step 5 : Step 5: Additional Packages Required
***
```
sudo apt-get install nodejs npm -y
sudo npm install -g rtlcss
```
***

## Step 6: Installation of wkhtmltox

****
To generate PDF reports in Odoo 16, wkhtmltopdf is required. Installing it is essential because PDF reports play a vital role in organizational operations.

Before installing wkhtmltopdf, we need to first install the `xfonts` dependency using the following command: 
***
***
After installing the required dependency, use the following set of commands to install wkhtmltox.
***

```
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.jammy_amd64.deb

sudo dpkg -i wkhtmltox_0.12.6.1-2.jammy_amd64.deb
```

## Step 7: Create a Log Directory and Provide Permissions

Odoo will keep tracking and recording audit trails. Create a dedicated log directory for Odoo and assign the necessary write permissions to it.

```
sudo mkdir /var/log/odoo

sudo chown odoo:odoo /var/log/odoo
```

## Step 8: Installation of Odoo 16
Once the above prerequisites are fulfilled, we are ready to proceed with the installation of Odoo 16. Use the following commands to download the Odoo 16 source code from the Git repository and complete the installation:

```
sudo apt-get install git

sudo git clone https://www.github.com/odoo/odoo /opt/odoo/odoo-server -b 16.0 --depth 1
```

## Step 9: Setup Required Permissions

After installing Odoo 16, grant it the required permissions so that it can function properly.

```
sudo chown -R odoo:odoo /opt/odoo/
```

### Step 10: Creation of a Server Configuration File

```
sudo touch /etc/odoo-server.conf

sudo su root -c "printf '[options] \n; This is the password that allows database operations:\n' | sudo tee /etc/odoo-server.conf"

sudo su root -c "printf 'admin_passwd = admin\n' | sudo tee /etc/odoo-server.conf"

sudo su root -c "printf 'xmlrpc_port = 9080\n' | sudo tee /etc/odoo-server.conf"

sudo su root -c "printf 'logfile = /var/log/odoo/odoo-server.log\n' | sudo tee /etc/odoo-server.conf"

sudo su root -c "printf 'addons_path=/opt/odoo/odoo-server/addons\n' | sudo tee /etc/odoo-server.conf"
```
```
sudo vim /etc/odoo-server.conf
```
Make changes as given header.
```
[DEFAULT]
addons_path = /opt/odoo/odoo-server/addons
...
```

Give the configuration file the appropriate permissions:

```
sudo chown odoo:odoo /etc/odoo-server.conf
sudo chmod 640 /etc/odoo-server.conf
```
The installation and execution of a programme can be separated using a Python virtual environment. Use the following instructions to build a virtual environment:

we must switch to the Odoo user by executing the command below:


```
sudo su -l odoo -s /bin/bash
cd odoo-server 

python3 -m venv odoo-venv  

source odoo-venv/bin/activate 
```

We must use the commands listed below to install the Python requirements into this virtual environment, which was made for our Odoo installation:

```
pip3 install wheel 
pip3 install -r requirements.txt
```

Using the deactivate command allows us to leave the virtual environment after installing Python dependencies:

```
deactivate
```

### Step 11: Start the Odoo Instance

Now that everything is set up

Execute the odoo-bin file pointing to the Odoo configuration file to activate the virtual environment before starting the Odoo service.

```
cd /opt/odoo/odoo-server

source odoo-venv/bin/activate

./odoo-bin -c /etc/odoo-server.conf
```


## Step 12: Login and Access the Odoo Application
***
We can customize the database after finishing the Odoo installation by going to the administrator page. Open any web browser and type http://localhost:8069 or http://_ip_or_domain:8069 into it to do this. A master password used during installation, a database name, a username, a password of your choice, and some basic information for account creation will then be required.

***
****
You will be sent to the Odoo login page at the same address, http://localhost:8069 or http://_ip_or_domain:8069, when the account has been created. Here, you can log in with the username and password you provided in the previous steps. You will then be able to access the home page. The Odoo setup is now finished, and you can add the applications of your choosing to the dashboard.
****
