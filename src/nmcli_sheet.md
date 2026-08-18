Here's a practical nmcli sheet for text-mode/TTY wifi, no GUI needed:

**Check status**

```
nmcli general status          # is NM running, connected?
nmcli device status           # list interfaces, state
nmcli connection show         # saved connections
nmcli connection show --active
```

**Connect to wifi**

```
nmcli device wifi list                          # scan/list networks
nmcli device wifi connect "SSID" password "PASS"  # connect (creates a saved profile)
```

**If NetworkManager itself seems dead/stuck (relevant mid-upgrade)**

```
sudo systemctl status NetworkManager
sudo systemctl restart NetworkManager
sudo systemctl status wpa_supplicant   # sometimes the actual culprit
```

**Re-use a saved profile (most common case after reboot)**

```
nmcli connection up "SSID"
```

**If the device shows unmanaged or disabled**

```
nmcli radio wifi on
nmcli device set wlan0 managed yes    # replace wlan0 with your actual iface name
```

**Get your interface name if unsure**

```
ip link show
# or
nmcli device status
```

**Delete a broken/stale profile and start clean**

```
nmcli connection delete "SSID"
nmcli device wifi connect "SSID" password "PASS"
```

**Quick connectivity sanity check**

```
ping -c 3 1.1.1.1     # network layer
ping -c 3 debian.org  # DNS + network
```

For your upgrade scenario specifically: after `sudo systemctl stop gdm3`, if wifi drops or acts weird, the sequence is usually `nmcli device status` → confirm wlan0 shows `disconnected` not `unavailable` → `nmcli connection up "SSID"`. If that fails, `sudo systemctl restart NetworkManager` first, then retry.