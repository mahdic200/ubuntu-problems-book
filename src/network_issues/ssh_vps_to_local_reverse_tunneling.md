Yes, absolutely. That's exactly what reverse SSH tunneling is for.

**On your local machine:**

```bash
ssh -L 9457:localhost:5432 mahdi@87.248.156.197 -p 9011 -N
```

This maps:

- Local port 9457 → SSH tunnel → VPS localhost:5432 (Postgres)

**Then in DB Visualizer:**

- Host: `localhost`
- Port: `9457`
- Username/Password: your Postgres credentials

Connect and you're done. You're now accessing the VPS's Postgres directly from your local machine through the encrypted tunnel.

**If Postgres is only listening on localhost on the VPS**, this works perfectly.

**If you need to keep multiple tunnels open** (the apt proxy + this Postgres tunnel):

```bash
# Terminal 1 - apt proxy tunnel (from earlier)
ssh -R 1080:192.168.1.6:8889 mahdi@87.248.156.197 -p 9011 -N

# Terminal 2 - Postgres tunnel
ssh -L 9457:localhost:5432 mahdi@87.248.156.197 -p 9011 -N
```

Both run simultaneously. You can open as many SSH tunnels as you need.

Try it out.