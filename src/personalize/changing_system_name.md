# changing system / host name

to change system name or host-name you just can simple enter this command :

```bash
sudo hostnamectl set-hostname <my_new_hostname>
```

example :

```bash
sudo systemctl set-hostname boyshostname
```

# sudo error after changing host-name

error after changing the hostname :

```bash
$ sudo su -
> sudo : unabe to resolve host boyshostname: name or service not known
```

Two things to check . `/etc/hostname` and `/etc/hosts` they should have the same name for your system .

`/etc/hostname` file contains just the machine name .
`/etc/hosts` has an entry for `localhost` . It should have something like
   ```conf
   127.0.0.1    localhost
   127.0.0.1    my-machine
```


