unable to mount PATH to /media/USER/PATH

# mount manually

find your drive name

```bash
sudo fdisk -l
```

```bash
sudo mkdir /media/USER/pick_a_name
```

then mount the drive

```bash
sudo mount FULL_PATH_OF_DRIVE /media/USER/picked_name
```

# unmount with umount

```bash
sudo umount /media/USER/picked_name
```

done :)

# Read only file system error
for ntfs file systems you should fix them because dual boot will harm your drive system file type :


```bash
sudo apt install ntfs-3g
```

```bash
sudo ntfsfix -b -d /path/to/drive
```