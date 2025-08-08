# psql configs

```
CREATE DATABASE yourdbname;
```

```
CREATE USER youruser WITH ENCRYPTED PASSWORD 'yourpass';
```

```
GRANT ALL PRIVILEGES ON DATABASE yourdbname TO youruser;
```

```
sudo -u postgres psql
\c EXAMPLE_DB postgres
GRANT ALL ON SCHEMA public TO EXAMPLE_USER;
```


