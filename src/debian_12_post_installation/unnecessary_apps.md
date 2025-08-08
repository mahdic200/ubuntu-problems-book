# remove unnecessary apps

# Removing Games and Stupid Gnome-Software

```shell
sudo apt purge aisleriot five-or-more four-in-a-row gnome-chess hitori gnome-klotski gnome-mahjongg gnome-mines gnome-nibbles gnome-robots gnome-sudoku gnome-taquin gnome-tetravex iagno lightsoff quadrapassel swell-foop tali gnome-2048
```

# Remove Stupid Gnome-Software

```shell
sudo apt remove --purge gnome-software*
```
# Remove Stupid Default apps

```shell
sudo apt remove --purge gnome-weather gnome-maps transmission*
```
# Remove Stupid tracker

```
sudo apt remove --purge tracker*
```

# Remove Stupid Evolution

First log out from GUI and then press `Ctrl + Alt + F3` or other `F` keys like `F5` , it doesn't really matter, we just want to switch to text mode .

Or you can simply tell the Linux to switch to text mode using :

```shell
sudo telinit 3
```

For coming back from text mode you can enter this command :

```shell
sudo telinit 5
```

To remove the evolution you can simply use this command :

```
sudo apt remove --purge evolution*
```

And then :

```shell
sudo apt autoremove --purge
```

# Remove GOA, Gnome Online Accounts

```shell
sudo apt remove gnome-online-accounts
```
# Remove XDG Portal

```shell
sudo apt remove --purge xdg-desktop-portal-gtk xdg-desktop-portal-gnome
```

# Remove Flatpak, Sandbox

```shell
sudo apt remove --purge flatpak*
```

