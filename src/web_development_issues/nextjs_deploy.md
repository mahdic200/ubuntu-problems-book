just use pm2

```shell
npm install -g pm2
```


starting a nextjs server:

navigate to your project folder and enter this :

```shell
sudo nano eco.config.js
```

```node
module.exports = {
  apps: [
    {
      name: "nextjs-app",         // Name of the application
      script: "npx",              // The script to execute
      args: "next start",         // Arguments for the script
      cwd: "./",                  // Current working directory
      env: {
        NODE_ENV: "production"    // Environment variables
      },
      // Set the maximum memory usage before restarting the application
      max_memory_restart: "300M"  // Restart the application if it uses more than 300MB of RAM
    }
  ]
}
```


listing processes :

```shell
pm2 ls
```

saving processes to execute in boot time :

```shell
pm2 save
```

monitoring :

```shell
pm2 monit
```

# connecting nodejs server to domain

replace the 3000 port with your nodejs server port :

```nginx.conf
server {
	# ... config
	location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
	# ... config
}
```

