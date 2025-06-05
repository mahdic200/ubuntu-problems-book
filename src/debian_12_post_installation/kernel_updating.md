# Kernel Updating

The first thing you must do for your very brand new laptop with a strong gpu is to update the kernel .

I don't know what version of debian are you using (bookworm, bullseye) but please apply the changes to the below commands relative to your distribution .


# Add the bookwom-backports source


```shell
sudo nano /etc/apt/sources.list
```

and add the following

```shell
deb http://deb.debian.org/debian bookworm-backports main contrib non-free non-free-firmware
```

then update the repositories :

```shell
sudo apt update
```

Now it's time to update the kernel relative to backports repository :

> [!WARNING]
> you must install all the dependencies relative to a very major update of your system . this is necessary take it serious .

```shell
sudo apt -t bookworm-backports upgrade
```


After this you can install `nvidia-driver` my dude .

This must fix a lot of things like suspend issue on new laptops .


