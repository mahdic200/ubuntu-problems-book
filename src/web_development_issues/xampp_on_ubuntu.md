# xampp on ubuntu

to install xampp on ubuntu and debian 12 you need to install this package first :

`sudo apt install net-tools`

and mysql-server this is a full reference [digital ocean](https://docs.vultr.com/how-to-install-mysql-on-debian-12) :

```bash
wget https://dev.mysql.com/get/mysql-apt-config_0.8.30-1_all.deb
```

```bash
sudo dpkg -i mysql-apt-config_0.8.30-1_all.deb
```

```bash
sudo apt update
sudo apt install mysql-server
```


and then you need to download the installation ***run*** file from apache friends website and download the latest version for ubuntu

then you need to install the installation file :

`sudo chmod 755 [package_name]`

then `sudo ./[package_name]`

for running xamp you need to create a sh file to do this code for you:

`sudo /opt/lampp/./manager-linux-x64.run`

# verifying PHP to ubuntu

```bash
sudo ln -s /opt/lampp/bin/php /usr/bin/php
```


### starting xampp in terminal

for starting xampp on ubuntu you have to enter some command like this :

`sudo ./xampp start`

for stopping xampp you have to do sth like this :

`sudo ./xampp stop`

### creating a systemd service for xampp

or you can create a service for xampp, create a new file :

`sudo nano /etc/systemd/system/xampp.service`

```conf
[Unit]
Description=XAMPP Service
After=network.target

[Service]
Type=forking
ExecStart=/opt/lampp/lampp start
ExecStop=/opt/lampp/lampp stop
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

save the file in nano `ctrl + X` and `Y` then Enter .

reload the systemd manager :

`sudo systemctl daemon-reload`

enable the service to start on boot `sudo systemctl enable xampp.service`

start the service using `sudo systemctl start xampp.service`

stop the service using `sudo systemctl stop xampp.service`

to check the status `sudo systemctl status xampp.service`