# docker.ir see this page

for iranian people , you must see this page and use it's mirror and proxy server . thank you :) , none of the previous steps are requierd .

just add docker.ir proxy server in `~/.docker/config.json` and add mirror to `/etc/docker/daemon.json` . FUCK IRAN AND FUCK DOCKER .



```bash
sudo mkdir -p /etc/systemd/system/docker.service.d
```

```bash
sudo touch /etc/systemd/system/docker.service.d/http-proxy.conf
```

```bash
sudo nano /etc/systemd/system/docker.service.d/http-proxy.conf
```

inside the .../http-proxy.conf:

> [!WARNING] HTTPS is not HTTPS !
> in some cases you have to put the http:// instead of https inside of HTTPS_PROXY variable . if proxy is not working try to do this .

```conf
[Service]
Environment="HTTP_PROXY=http://example.com:3129"
Environment="HTTPS_PROXY=https://example.com:3129"
# Environment="NO_PROXY=localhost,127.0.0.1
```

> [!IMPORTANT] reload the docker
> in some cases like this you have to reload the docker to new settings be loaded in program and work correctly .

```bash
sudo systemctl daemon-reload && \
sudo systemctl restart docker
```

verify completed everything fine :

```bash
sudo systemctl show --property=Environment docker
```

you should see something like this:

```
Environment=HTTP_PROXY=http://example.com:3129 HTTPS_PROXY=http://example.com:3129
```
