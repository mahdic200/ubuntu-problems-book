# debian suspend crashing

there is a problem with debian , when you suspend your laptop you have to close and open the lid to be able to wake it , or laptop just tries to turn on monitor but refuses to do this and after hard resetting the system you will see thousands of error logs with `journalctl | grep -i suspend` command . how to fix it ? follow this note .

## remove nvidia drivers

there is nothing wrong with debian , you probably have installed a bunch of nvidia drivers which is a really bad move . you have to uninstall all of them .

```bash
sudo apt remove --purge nvidia*
```

for me , it fixed the issue .