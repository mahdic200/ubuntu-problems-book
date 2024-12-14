# mysql installation

digital ocean is the best : [https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04) 

```bash
sudo apt update
```

```bash
sudo apt install mysql-server
```

```bash
sudo systemctl start mysql.service
```

```bash
sudo mysql
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

exit mysql :

```sql
exit
```

```bash
mysql -u root -p
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH auth_socket;
```

```bash
sudo mysql_secure_installation
```

```sql
CREATE USER 'username'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

to grant a **single** privilege to a user for a database's table :

```sql
GRANT PRIVILEGE ON database.table TO 'username'@'localhost';
```

to grant all privileges for all databases to a user :

```sql
GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' WITH GRANT OPTION;
```

```sql
FLUSH PRIVILEGES;
```


