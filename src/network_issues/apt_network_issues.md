# nothing works for apt update Ign Error !

if nothing works for your problem just open a terminal and run this  command :

```bash
sudo nano /etc/apt/apt.conf
```

and write this line of code inside of apt.conf file :

```conf
Acquire::http::Proxy "http://yourproxyaddress:proxyport";
```


go in software and updates . then click on select-box "download from" , click on "other ..." . and click on select best server in popped up window . wait to test complete .

enter this command:

```bash
code /etc/apt/sources.list.d/
```

and open `ubuntu.sources` file .

comment this section:

```conf
# Types: deb
# URIs: http://security.ubuntu.com/ubuntu/
# Suites: noble-security
# Components: main restricted universe multiverse
# Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```