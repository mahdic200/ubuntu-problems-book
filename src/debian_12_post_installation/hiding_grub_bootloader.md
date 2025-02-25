# hiding grub bootloader

open up your `/etc/default/grub` :

```bash
sudo nano /etc/default/grub
```

and change the `GRUB_TIMEOUT` to `0` and add the `GRUB_HIDDEN_TIMEOUT_QUIET=true` after it.

now update the boot-loader :

```bash
sudo update-grub
```

now after rebooting you won't see the grub menu , just a simple graphical verbose which indicates which kernel version is being loaded .
