# Global network proxy

since we are Iranian people and we have a lot of network issues (limits , 403 errors etc.) we have to set a proxy that works for all of our applications in Linux systems , and though Linux systems does not provide such a thing , I have a big boom solution !! .

## use your phone's hotspot !

the only thing you need is to install `VPN Hotspot` application which it's Icon is a green key (for now of course) . your phone has to be rooted (if you don't have such an option please use USB tethering on your android phone) . and with this method we can have a good global proxy system which is a trick . and don't remember to have these things on your mind :
- turn on your android VPN
- use New York (United States) IP address and timezone
- don't forget to use your network when Internet connection is strong
- make sure your Linux system is using your phones VPN (you can check your IP address)
that must does the trick .

## weak alternative with sshuttle

This is a very weak alternative but may work for you .
```bash
sudo apt install sshuttle
```

```bash
sudo sshuttle --dns -x <server_ip> -r <username>@<server_ip>:<port> 0/0
```

verify your proxy settings are applied :

```bash
curl ip-api.com
```
