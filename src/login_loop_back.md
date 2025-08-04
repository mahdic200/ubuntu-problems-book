# login loop back

just remove the fucking `.config/gnome-session` directory .


These information maybe needed as well .
### Command Cheat Sheet

#### Booting and System State

- Switch to Text Mode Permanently:
    
    sudo systemctl set-default multi-user.target
    
- Switch to Graphical Mode Permanently:
    
    sudo systemctl set-default graphical.target
    
- Start Graphical Session (Instant):
    
    startx
    
- Start Graphical Session (via systemd):
    
    sudo systemctl start graphical.target
    
- Reboot the System:
    
    sudo reboot
    

#### Networking (Wi-Fi)

- Check Wi-Fi Device Name:
    
    nmcli dev status
    
- Scan for Networks:
    
    nmcli dev wifi list
    
- Enable Wi-Fi Radio:
    
    nmcli radio wifi on
    
- Connect to a Network:
    
    nmcli dev wifi connect "SSID" password "your_password" ifname DeviceName
    

#### Troubleshooting

- Reset GNOME Configuration:
    
    dconf reset -f /
    
- Reinstall GDM3:
    
    sudo apt install --reinstall gdm3
    
- Fix the Gnome Keyring (the final fix):
    
    rm -f ~/.local/share/keyrings/login.keyring
    
- View System Errors:
    
    journalctl -b -p err