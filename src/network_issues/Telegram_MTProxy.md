# Telegram MTProxy

this article is a copy of [this article](https://gist.github.com/rameerez/8debfc790e965009ca2949c3b4580b91?permalink_comment_id=4999721) .

# How to set up a Telegram Proxy (MTProxy)

This tutorial will teach you how to set up a Telegram MTProxy on an Ubuntu 22.04 sever using AWS Lightsail, although you can use any other Linux distribution and cloud provider.

Using a Telegram proxy is a safe, easy and effective way of overcoming Telegram bans. It's useful, for example, to keep using Telegram under tyrannical regimes, or to circumvent judges' decisions to block Telegram.

Telegram proxies are a built-in feature in all Telegram apps (both mobile and desktop). It allows Telegram users to connect to a proxy in just one or two clicks / taps.

Telegram proxies are safe: Telegram sends messages using their own [MTProto](https://core.telegram.org/mtproto) secure protocol, and the proxy can only see encrypted traffic – there's no way for a proxy to decrypt the traffic and read the messages. The proxy does not even know which Telegram users are using the proxy, all the proxy sees is just a list of IPs.

This guide exists because the README in the official [TelegramMessenger/MTProxy](https://github.com/TelegramMessenger/MTProxy) repo is both incomplete and hasn't been updated for the past 5 years, and fails at multiple points if you try following the steps described. This is an updated version as of March 2024. There's also an official Docker image, but it's also outdated.

I've used these exact steps to set multiple proxies myself, including [my public Telegram proxy](https://t.me/proxy?server=telegramproxy.rameerez.com&port=8443&secret=dca82c6c73f2dbc3ca8e9045ee760283) that you can use if you don't want to go through the hassle of setting up your own.

## Instructions

To begin, launch a clean Ubuntu 22.04 instance. I'm using [AWS Lightsail](https://lightsail.aws.amazon.com). I chose a $3.5/mo instance (512MB, 2vCPUs, 1TB transfer), which should be enough for non-intensive usage. You can choose a bigger instance, or set up your server in any other cloud provider (DigitalOcean, Linode, Hetzner, etc) based on your needs and preferences. Note that the physical location you choose for the instance has to be a place where Telegram is not banned / restricted for the proxy to work.

1. `ssh` into the machine you've just launched:
```
ssh ubuntu@ip
```

2. Update apt:
```
sudo apt-get update
```

3. Install dependencies:
```
sudo apt install git curl build-essential libssl-dev zlib1g-dev
```

4. Clone the repo:

We're going to use an unofficial [MTProxy community fork](https://github.com/GetPageSpeed/MTProxy) instead of [the official one](https://github.com/TelegramMessenger/MTProxy). Why? The official Telegram MTProxy repo is considered abandoned: it hasn't been updated for many years. Many problems that need fixing haven't been fixed, and if you try using the official repo code, [MTProxy will unexpectedly break in production](https://gist.github.com/rameerez/8debfc790e965009ca2949c3b4580b91?permalink_comment_id=5004249#gistcomment-5004249). This fork is a community effort to keep things up to date so you can keep running MTProxy in production without surprises.
```
git clone https://github.com/GetPageSpeed/MTProxy
cd MTProxy
```

5. Change the `Makefile` and add the `-fcommon` flag to `CFLAGS` and `LDFLAGS` as per [this PR](https://github.com/TelegramMessenger/MTProxy/pull/531)
```
nano Makefile
```
Save and exit

6. Build the binaries
```
make
```
Make sure it compiles without errors.

7. Move the binary to `/opt/MTProxy` for ease of running:
```
sudo mkdir /opt/MTProxy
sudo cp objs/bin/mtproto-proxy /opt/MTProxy/
```

8. Go to the new directory:
```
cd /opt/MTProxy
```

9. Obtain the Telegram secret:
```
sudo curl -s https://core.telegram.org/getProxySecret -o proxy-secret
```

10. Obtain the Telegram configuration:
```
sudo curl -s https://core.telegram.org/getProxyConfig -o proxy-multi.conf
```

11. Generate a proxy secret. This will output a string of random numbers and letters. Keep the result at hand, you will need it in a few steps:
```
head -c 16 /dev/urandom | xxd -ps
```

12. Create a `mtproxy` user to run the proxy:
```
sudo useradd -m -s /bin/false mtproxy
```

13. Update the ownership of the MTProxy directory to the new user
```
sudo chown -R mtproxy:mtproxy /opt/MTProxy
```

14. Allow traffic on port 8443 by opening the ports in the AWS Lightsail instance:
    - Navigate to your AWS Lightsail instance
    - In the Networking tab, under "IPv4 Firewall", click "Add rule"
    - Add a rule for a "Custom" TCP protocol on 8443. Make sure "Duplicate rule for IPv6" is active
    - Click "create"
    - Since you're at it: close port 80, which is open by default, because we're not going to use it

If your instance uses the `ufw` firewall, you will also need to do:
```
sudo ufw allow 8443/tcp
```

15. Now we need to know our AWS instance's private and public IP to pass them to MTProxy.

All AWS instances are behind a NAT, and this causes the RPC protocol handshake to fail if a private-to-public network address translation is not passed explicitly to MTProxy as the `--nat-info` param. If you don't do this, the proxy will look like it's running normally, but Telegram clients will not be able to connect, and the app will show a message like "Proxy unavailable" or an infinite "Conecting..." message.

If you don't know how to look up your AWS instance's public and private IPs, follow [these steps](https://github.com/TelegramMessenger/MTProxy/issues/194#issuecomment-787639858)

Once you have your private and public IP, which should look something like `170.10.0.200` and `18.180.0.1`, keep them at hand because you'll need them in a moment and continue.


16. Set up a `systemd` service to run the proxy:
```
sudo nano /etc/systemd/system/MTProxy.service
```

Copy the folliwng config, make sure you edit it with your own params:
```
[Unit]
Description=MTProxy
After=network.target

[Service]
Type=simple
WorkingDirectory=/opt/MTProxy
ExecStart=/opt/MTProxy/mtproto-proxy -u mtproxy -p 8888 -H 8443 -S <YOUR_SECRET_FROM_STEP_11> --aes-pwd proxy-secret proxy-multi.conf -M 1 --http-stats --nat-info <YOUR_PRIVATE_IP>:<YOUR_PUBLIC_IP>
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Save and exit

17. Reload the systemd daemons:
```
sudo systemctl daemon-reload
```

18. Test the MTProxy service and verify it started just fine:
```
sudo systemctl restart MTProxy.service
```

After that check the proxy status, it should be active:
```
sudo systemctl status MTProxy.service
```

The proxy is ready!

You should now be able to connect to it inside Telegram by using a link like:
```
tg://proxy?server=<YOUR_PUBLIC_IP>&port=8443&secret=<YOUR_SECRET_FROM_STEP_11>
```

Or share it literally anywhere by using an HTTP link:
```
https://t.me/proxy?server=<YOUR_PUBLIC_IP>&port=8443&secret=<YOUR_SECRET_FROM_STEP_11>
```

If you want to enable **random padding** in your client, add `dd` at the beggining of your secret (read more in the [official MTProxy README](https://github.com/TelegramMessenger/MTProxy/blob/master/README.md#random-padding))

Example of a proxy link with random padding enabled client-side:
```
https://t.me/proxy?server=<YOUR_PUBLIC_IP>&port=8443&secret=dd<YOUR_SECRET_FROM_STEP_11>
```

19. Make sure you enable the service so the MTProxy starts even if you reboot the machine:
```
sudo systemctl enable MTProxy.service
```

20. Set up a cron job to update `proxy-multi.conf` on a daily basis. Telegram [recommends](https://github.com/TelegramMessenger/MTProxy?tab=readme-ov-file#running) that proxies update their Telegram config information at least once a day, since it may change. To do that with the right permissions, we need to set up a cron job as the root user:

 - Switch to the root user:
```
sudo su
```

 - Open the root user's crontab file for editing:
```
crontab -e
```
If prompted, choose an editor (e.g., nano) to edit the crontab file.

 - In the crontab file, add the following line at the end. This will set up the update task to be run every day at 4am. Since the proxy might experience a very short downtime while restarting the MTProxy service, choose a time (usually at night) that minimizes the effects of downtime.
```
0 4 * * * curl -s https://core.telegram.org/getProxyConfig -o /opt/MTProxy/proxy-multi.conf && chown -R mtproxy:mtproxy /opt/MTProxy && systemctl restart MTProxy.service
```
Save and exit the root session.

---

✨ Congrats!

Your proxy is now all set and will continue to be updated automatically!

You can leave it here – but there's more you can do, if you want.

For example, Telegram rewards people that set up proxies by allowing them to promote a channel of their choice to all users connected to the proxy. This channel shows up in the top of their chat list labeled as "Proxy Sponsor".

If you want to register your proxy to get usage statistics and set a promoted channel to proxy users, talk with Telegram's official [MTProxybot](https://t.me/MTProxybot) – it will give you a `tag` you can pass to `mtproto-proxy` with the flag `-P` in the systemd config from step 16, like this:
```
ExecStart=/opt/MTProxy/mtproto-proxy -u mtproxy -p 8888 -H 8443 -S <YOUR_SECRET_FROM_STEP_11> -P <YOUR_MTPROXYBOT_TAG> --aes-pwd proxy-secret proxy-multi.conf -M 1 --http-stats --nat-info <YOUR_PRIVATE_IP>:<YOUR_PUBLIC_IP>
```

This way MTProxyBot can keep track of your proxy's requests to compile stats and set the promoted channel.

As a finishing touch, you can also use your own domain/subdomain instead of your public instance's IP, for more readable proxy names and URLs, like what I did with my proxy `telegramproxy.rameerez.com`:
```
https://t.me/proxy?server=telegramproxy.rameerez.com&port=8443&secret=dca82c6c73f2dbc3ca8e9045ee760283
```

To do this, just set up a DNS "A" record pointing to your proxy's IP. If you're using Cloudflare, make sure to turn off the proxy feature for that particular record. Oddly enough, the domain/subdomain you use can't contain hyphens; just stick to alphanumeric characters.

Thanks for reading and happy proxying!