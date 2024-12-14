```bash
sudo apt install sshuttle
```

```bash
sudo sshuttle --dns -x <server_ip> -r <username>@<server_ip>:<port> 0/0
```

verify your proxy settings are applied :

```bash
curl ip-api.com
```
