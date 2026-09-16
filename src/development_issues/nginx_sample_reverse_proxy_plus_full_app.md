
```nginx
# /etc/nginx/sites-available/default (or your site config)

upstream next_app {
    server localhost:3000;
}

upstream fiber_backend {
    server localhost:8000;
}

server {
    listen 80;
    server_name yourdomain.com; # or localhost for dev

    # API proxy
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

    # Uploads proxy
    location /uploads/ {
        proxy_pass http://fiber_backend;
        proxy_http_version 1.1;
        
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Host $server_name;
        
        proxy_set_header Host $host;
        proxy_set_header Connection "";
    }

    # Everything else goes to Next.js
    location / {
        proxy_pass http://next_app;
        proxy_http_version 1.1;
        
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```