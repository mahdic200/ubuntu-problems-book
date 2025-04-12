# Graphics card driver switching

in Debian 12 when you install `NVIDIA` driver your `initramfs` will be updated by `/etc/modprobe.d/nvidia-blaklists-nouveau.conf` file .

so you won't be able to use `nouveau` driver anymore , such a problem , isn't it ? . but here's that my solution comes out !

## Selecting Nouveau

open `/etc/modprobe.d/` folder with your VSCode or any editor you want (and be able to save file as root) . open `nvidia.conf` file and comment out all of it , first by selecting all content with `Ctrl + A` and then commenting with `Ctrl + /` and then save .

now open `nvidia-blacklists-nouveau.conf` file and do the same . you may notice that there is a comment line on top of the file , it's telling something important ! you have to update the `initramfs` file with this command :

```bash
sudo update-initramfs -u
```

it is really important to do this because this is related to your bootable file which loads the kernel , and kernel modules are loaded at boot time so you have and must to update this file after any major changes or driver switching like this .

now you can close any application and reboot your system safely :

```bash
sudo systemctl reboot
```

## Checking the Loaded Driver

after a complete reboot you can check whether your driver is loaded or not :

```bash
sudo lspci -k | grep -A 3 -i "VGA"
```

and you should see something like this :

```
00:02.0 VGA compatible controller: Intel Corporation Alder Lake-P GT1 [UHD Graphics] (rev 0c)
	Subsystem: Lenovo Alder Lake-P GT1 [UHD Graphics]
	Kernel driver in use: i915
	Kernel modules: i915
--
01:00.0 VGA compatible controller: NVIDIA Corporation GA107M [GeForce RTX 3050 Ti Mobile] (rev a1)
	Subsystem: Lenovo GA107M [GeForce RTX 3050 Ti Mobile]
	Kernel driver in use: nouveau
	Kernel modules: nouveau, nvidia_current_drm, nvidia_current
```

## Selecting nvidia

there is nothing special about reversing those operations back :) . just open the `/etc/modprobe.d/` directory with code or any desired editor and uncomment the contents of these files `nvidia-blacklists-nouveau.conf` and `nvidia.conf` . then enter this command again :

```bash
sudo update-initramfs -u
```

and then reboot :) .