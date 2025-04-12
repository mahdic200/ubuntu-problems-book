clear DNS cache:

```bash
sudo resolvectl flush-caches
```

problem to installing packages:

```bash
sudo apt update
``` 

```bash
sudo apt-get update
```

if net not working for getting apt packages : 

```bash
sudo nano /etc/resolv.conf
```


and add these lines before nameserver ... :

```conf
nameserver 8.8.8.8
nameserver 8.8.4.4
```



solve apt update network issue:

```bash
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf > /dev/null
```