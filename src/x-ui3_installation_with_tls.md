# X-UI3 installation with tls

## ✅ Setting up `3x-ui` Panel with TLS (via Certbot & Nginx) — Clean Guide

### **1. Download & Install 3x-ui**

* Clone or download the **3x-ui** panel from [Mohammad Hossein Sanaei’s GitHub](https://github.com/mhsanaei/3x-ui).
* Install it using the provided script.

### **2. Disable the Firewall Temporarily**

* Temporarily disable `ufw` or `firewalld` to avoid conflicts during the setup phase:

```bash
sudo ufw disable
```

### **3. Run the Panel Without SSL on Raw IP**

* Start the panel and **check its status** using:

```bash
3x-ui status
```

* Open it in the browser via your **server's raw IP and port** (default: `2053`) to ensure it’s working.

### **4. Test Basic Functionality**

* Create a **basic config** inside the panel — no TLS, no subdomain — just to ensure everything works before adding complexity.

### **5. Re-enable Firewall & Whitelist Ports**

* Add the panel and SSH ports to the firewall:

```bash
sudo ufw allow 22
sudo ufw allow 2053
sudo ufw enable
```

* Verify the panel is still accessible via raw IP and port.

### **6. Set Up Domain & Nginx Reverse Proxy**

* Create a subdomain (e.g., `panel.yourdomain.com`) and point its A record to your server IP.
* Configure Nginx to reverse proxy to your panel:

#### **Main path (`/`) reverse proxy config**:

```nginx
location / {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Range $http_range;
    proxy_set_header If-Range $http_if_range; 
    proxy_redirect off;
    proxy_pass http://127.0.0.1:2053;
}
```

#### **Optional sub path (`/sub`) reverse proxy**:

```nginx
location /sub {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Range $http_range;
    proxy_set_header If-Range $http_if_range; 
    proxy_redirect off;
    proxy_pass http://127.0.0.1:2053;
}
```

### **7. Obtain SSL Certificate Using Certbot**

* Install Certbot and get the certificate:

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d panel.yourdomain.com
```

* Certbot will generate two important files:

  * `fullchain.pem` (public key)
  * `privkey.pem` (private key)

### **8. Update TLS Settings in 3x-ui Panel**

* Go to `Config` → `Edit`
* Enable TLS.
* Select **“File Content”** instead of “File Path”.
* Copy the **ENTIRE** content (even comments or extra lines matter):

  * Paste the **fullchain.pem** content into the **Public Key** field.
  * Paste the **privkey.pem** content into the **Private Key** field.

### **9. Configure SNI in Inbound Settings (Not Outbound)**

* In your **3x-ui panel**, go to the **left sidebar > Inbounds**.
* Edit the inbound config you're using (e.g., VLESS, Trojan).
* Set the **SNI (Server Name Indication)** to your domain:

  ```
  panel.yourdomain.com
  ```
* This ensures the TLS handshake is correctly matched to your domain during client connections.

### **10. Whitelist Additional Config Ports in Firewall**

* Allow any additional inbound ports (for VMess/VLESS, Trojan, etc.) in your firewall.
