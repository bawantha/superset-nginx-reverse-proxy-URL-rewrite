# superset-nginx-reverse-proxy-URL-rewrite
# QUICK HACK FOR REMOVE /superset base URL

under ```/etc/nginx/sites-availble```

add new .conf file or change ```default.conf``` 

```
upstream superset {
    server localhost:9000;  // dev-server
}

server {
    listen 80;

    location ~ ^/superset/ {
        rewrite ^/superset(.*)$ http://$host/analytics$1 permanent;
    }


    location / {
        rewrite ^/analytics(.*) /superset$1 break;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Script-Name /analytics;
        proxy_pass http://superset;
        proxy_redirect off;
    }

}


```
