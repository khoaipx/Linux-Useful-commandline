Normally, our service will look like: host:port/servicePath/?abcxyz

We will want the service should look better: host/servicePath/?abcxyz

If your server run nginx, here is what you can do

### Back up file /etc/nginx/site-available/default
```
sudo cp /etc/nginx/site-available/default /etc/nginx/site-available/default.bak
```

### Edit file /etc/nginx/site-available/default
```
sudo vim /etc/nginx/site-available/default
```

You will see something like this
```
server {
        listen 80 default_server;
        # listen [::]:80 default_server ipv6only=on;

        root /usr/share/nginx/html;
        index index.html index.htm;

        # Make site accessible from http://localhost/
        server_name _;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
                # Uncomment to enable naxsi on this location
                # include /etc/nginx/naxsi.rules
        }
 ...
}
```

### Add your config

Let's add your config to server's block
```
        location /your_service_path {
                proxy_pass http://localhost:your_service_host;
        }
```

### Save the config file, then check if you config nginx correctly or not

It needs to be like this
```
ubuntu@ip-172-31-23-188:~$ sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

### If nginx config test passed, restart nginx to run with your config
```
sudo nginx -s reload
```

### Config nginx for messageClassify at classify.finsify.com (52.39.47.142)

Instead of */etc/nginx/site-available/default*, edit file:

```
/etc/nginx/conf.d/classify.finsify.conf
```