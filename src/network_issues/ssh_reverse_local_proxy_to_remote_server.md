Yes, exactly! That's perfect. You can tunnel your phone's proxy to the VPS via SSH.

  

**On your local machine, run:**

  

```bash

ssh -R 1080:192.168.1.6:8889 mahdi@87.248.156.197 -p 9011 -N

```

  

This creates a reverse tunnel:

- Port 1080 on the VPS connects back to your local machine

- Your local machine forwards it to 192.168.1.6:8889 (your phone's proxy)

  

**On the VPS, test it:**

  

```bash

curl -x http://localhost:1080 https://www.google.com

```

  

(Use `http://` not `socks5h://` since your phone's everyproxy is HTTP, not SOCKS)

  

If you get Google's HTML, **it works**—the VPS is using your phone's VPN to access the internet.

  

**Then for apt:**

  

```bash

sudo apt -o Acquire::http::Proxy="http://localhost:1080" update

```

  

Keep the SSH tunnel running in the background on your local machine, and the VPS will have internet access through your phone's VPN.

  

Does this work?