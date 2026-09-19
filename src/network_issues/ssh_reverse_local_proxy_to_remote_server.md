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

The remote port 1080 is already in use on the VPS. Either:

**Option 1: Use a different port**

```bash
ssh -i ~/.ssh/id_ed25519myvps -R 2080:192.168.1.6:8889 mahdi@87.248.156.197 -p 9011 -N
```

Then on VPS use `localhost:2080` for the proxy.

**Option 2: Kill the process using port 1080 on VPS**

```bash
# On VPS, find what's using port 1080
sudo lsof -i :1080
# or
sudo ss -tlnp | grep 1080

# Kill it
sudo kill -9 <PID>
```

Then try the original command again.

**Option 3: Check if there's an old SSH tunnel still running**

On your local machine:

```bash
ps aux | grep ssh
# Look for the reverse tunnel command and kill it
kill <PID>
```

Then start the tunnel again.

I'd recommend **Option 1** (use a different port like 2080) to avoid conflicts.