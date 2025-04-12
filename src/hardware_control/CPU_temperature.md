# CPU temperature

```bash
sudo apt install lm-sensors
```

```bash
sudo sensors-detect
```

answer yes to all .
```bash
sudo service kmod start
```

then :
```bash
sensors
```