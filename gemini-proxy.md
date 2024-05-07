# Google Gemini API Reverse Proxy Config
---
```
server {
    listen 80;
    server_name proxy.example.com;
    location / {
        rewrite ^/(.*)$ https://$host$1 permanent;
    }
}

server {
    listen 443 ssl;
    server_name proxy.example.com;

    ssl_certificate  /etc/letsencrypt/live/proxy.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/proxy.example.com/privkey.pem;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    location /googleai {
        rewrite /googleai/(.*) /$1 break;
        proxy_pass https://generativelanguage.googleapis.com;
        proxy_set_header Host generativelanguage.googleapis.com;

        proxy_set_header User-Agent $http_user_agent;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Accept-Encoding "";
        proxy_set_header Accept-Language $http_accept_language;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
