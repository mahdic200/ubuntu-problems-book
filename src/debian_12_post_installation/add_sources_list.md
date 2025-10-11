# add sources list

enter command `sudo nano /etc/apt/sources.list` then paste these lines down there : 

```
# deb cdrom:[Debian GNU/Linux 12.7.0 _Bookworm_ - Official amd64 DVD Binary-1 with firmware 20240831-10:40]/ bookworm contrib main non-free-firmware

deb https://deb.debian.org/debian trixie main non-free non-free-firmware
deb-src https://deb.debian.org/debian trixie main non-free non-free-firmware

deb https://security.debian.org/debian-security trixie-security main non-free non-free-firmware
deb-src https://security.debian.org/debian-security trixie-security main non-free non-free-firmware

deb https://deb.debian.org/debian trixie-updates main non-free non-free-firmware
deb-src https://deb.debian.org/debian trixie-updates main non-free non-free-firmware
```


## Testing Sources List
```
deb https://deb.debian.org/debian testing main non-free non-free-firmware
deb-src https://deb.debian.org/debian testing main non-free non-free-firmware

deb https://security.debian.org/debian-security testing-security main non-free non-free-firmware
deb-src https://security.debian.org/debian-security testing-security main non-free non-free-firmware

deb https://deb.debian.org/debian testing-updates main non-free non-free-firmware
deb-src https://deb.debian.org/debian testing-updates main non-free non-free-firmware
```