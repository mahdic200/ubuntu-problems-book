```nginx
upstream next_app {

server localhost:3000;

}

  

upstream fiber_backend {

server localhost:8000;

}

  

# HTTP server - redirect all to HTTPS

server {

listen 80;

listen [::]:80;

server_name shafafgilan.ir www.shafafgilan.ir;

location / {

return 301 https://$host$request_uri;

}

}

  

# HTTPS server

server {

listen 443 ssl http2;

listen [::]:443 ssl http2;

server_name shafafgilan.ir www.shafafgilan.ir;

# SSL certificates (managed by Certbot)

ssl_certificate /etc/letsencrypt/live/shafafgilan.ir/fullchain.pem;

ssl_certificate_key /etc/letsencrypt/live/shafafgilan.ir/privkey.pem;

ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

# SSL configuration

ssl_protocols TLSv1.2 TLSv1.3;

ssl_ciphers HIGH:!aNULL:!MD5;

ssl_prefer_server_ciphers on;

access_log /var/log/nginx/shafafgilan.ir_access.log;

error_log /var/log/nginx/shafafgilan.ir_error.log;

# API proxy to Fiber backend

location /api/ {

proxy_pass http://fiber_backend;

proxy_http_version 1.1;

# Forward real client IP

proxy_set_header X-Forwarded-For $remote_addr;

proxy_set_header X-Forwarded-Proto $scheme;

proxy_set_header X-Forwarded-Host $server_name;

# Keep original headers

proxy_set_header Host $host;

proxy_set_header Connection "";

}

# Uploads proxy to Fiber backend

location /uploads/ {

proxy_pass http://fiber_backend;

proxy_http_version 1.1;

proxy_set_header X-Forwarded-For $remote_addr;

proxy_set_header X-Forwarded-Proto $scheme;

proxy_set_header X-Forwarded-Host $server_name;

proxy_set_header Host $host;

proxy_set_header Connection "";

}

# Everything else goes to Next.js frontend

location / {

proxy_pass http://next_app;

proxy_http_version 1.1;

proxy_set_header Upgrade $http_upgrade;

proxy_set_header Connection "upgrade";

proxy_set_header Host $host;

proxy_set_header X-Real-IP $remote_addr;

proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

proxy_set_header X-Forwarded-Proto $scheme;

proxy_cache_bypass $http_upgrade;

}

}
```

# Increasing client max request size

Add this after error log:

```
# Allow 30MB requests

client_max_body_size 30M;
```

## For a specific api endpoint with proxy pass

```
# Specific endpoint - allow 30MB for book uploads

location /api/admin/study/book/store {

client_max_body_size 30M;

proxy_pass http://fiber_backend;

}
```

