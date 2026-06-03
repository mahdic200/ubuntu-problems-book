navigate to your `~/.local/bin` folder :

```bash
cd ~/.local/bin
```

then make two files named `nson` and `nsoff` respectively :

```bash
touch nson nsoff
```

then give them permission to become executable :

```bash
chmod +x nson nsoff
```


# nson file content

```bash
#!/bin/bash

# Variables for DNS addresses
DNS1="185.51.200.2"
DNS2="178.22.122.100"

# Get the name of the WiFi interface
WIFI_INTERFACE=$(nmcli device status | grep wifi | awk '{print $1}')

# Get the name of the active WiFi connection
WIFI_CONNECTION=$(nmcli -t -f active,ssid dev wifi | grep '^yes' | cut -d: -f2)

# Set the DNS for the WiFi connection
nmcli con mod "$WIFI_CONNECTION" ipv4.dns "$DNS1 $DNS2"
nmcli con mod "$WIFI_CONNECTION" ipv4.ignore-auto-dns yes

# Restart the network connection to apply changes
nmcli con down "$WIFI_CONNECTION" && nmcli con up "$WIFI_CONNECTION"

echo "DNS for WiFi connection '$WIFI_CONNECTION' set to $DNS1 and $DNS2."

```

# nsoff file content

```bash
#!/bin/bash

# Get the name of the WiFi interface
WIFI_INTERFACE=$(nmcli device status | grep wifi | awk '{print $1}')

# Get the name of the active WiFi connection
WIFI_CONNECTION=$(nmcli -t -f active,ssid dev wifi | grep '^yes' | cut -d: -f2)

# Clear the DNS settings for the WiFi connection
nmcli con mod "$WIFI_CONNECTION" ipv4.dns ""
nmcli con mod "$WIFI_CONNECTION" ipv4.ignore-auto-dns no

# Restart the network connection to apply changes
nmcli con down "$WIFI_CONNECTION" && nmcli con up "$WIFI_CONNECTION"

echo "DNS settings for WiFi connection '$WIFI_CONNECTION' have been cleared."

```

---------

# ChatGPT's New Advice

You **do have NetworkManager**, and it is managing your Wi‑Fi/DNS. So don’t edit `/etc/resolv.conf` by hand; it may be overwritten.

Use `nmcli` to set DNS for your connection.

## 1) Find your active connection name
```bash
nmcli connection show --active
```

Note the `NAME` for your Wi‑Fi, likely something like `mahdic200`.

## 2) Set custom DNS for that connection
Replace `YOUR_CONN` with the exact connection name:
```bash
sudo nmcli connection modify "YOUR_CONN" ipv4.dns "1.1.1.1 8.8.8.8"
sudo nmcli connection modify "YOUR_CONN" ipv4.ignore-auto-dns yes
```

If you also want IPv6 DNS:
```bash
sudo nmcli connection modify "YOUR_CONN" ipv6.dns "2606:4700:4700::1111 2001:4860:4860::8888"
sudo nmcli connection modify "YOUR_CONN" ipv6.ignore-auto-dns yes
```

## 3) Reconnect
```bash
sudo nmcli connection down "YOUR_CONN"
sudo nmcli connection up "YOUR_CONN"
```

If it’s your only network and `down` disconnects awkwardly, you can instead do:
```bash
sudo nmcli device reapply wlp0s20f3
```
or just reconnect from the network menu.

## 4) Verify
```bash
nmcli device show wlp0s20f3 | grep DNS
```

Also:
```bash
resolvectl status
```
or
```bash
cat /etc/resolv.conf
```

## 5) Restore automatic DNS later
```bash
sudo nmcli connection modify "YOUR_CONN" ipv4.ignore-auto-dns no
sudo nmcli connection modify "YOUR_CONN" ipv4.dns ""
sudo nmcli connection up "YOUR_CONN"
```

## Notes
- This is **per-connection**, not per-user.
- If your Wi‑Fi name changes or you use another hotspot/router, you may need to set DNS on that connection too.

If you want, I can give you the exact commands after you paste:
```bash
nmcli connection show --active
```