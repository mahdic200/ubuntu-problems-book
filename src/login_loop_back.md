# login loop back

If you try to source any files that don't exist in `~/.profile` file, your GUI gnome will stuck in a looped login trap . if this ever happened to you do this :
1. press `Ctrl + Alt + F2`
2. After seeing the terminal , login to your user in text mode
3. then open the `.profile` with nano or any editor you like
4. search for any file that doesn't exist and tried to be accessed or sourced .
5. delete it, and exit from the terminal session
6. press `Ctrl + Alt + F1` to get back to GUI, now login
Your welcome .

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