# firewall tutorial

digital ocean : [https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands)


## inspection

```bash
sudo ufw status
```

## enable

```bash
sudo ufw enable
```

# disable

```bash
sudo ufw disable
```

## Block an IP Address

```bash
sudo ufw deny from 203.0.113.100
```

# Delete a rule

```bash
sudo ufw delete allow from 203.0.113.101
```

## list available application profiles

```bash
sudo ufw app list
```

## enable application profile

```bash
sudo ufw allow "OpenSSH"
```

## disable application profile

example :

```bash
sudo ufw delete allow 22/tcp
```
