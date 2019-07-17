# Laravel WebSockets Demo 🛰

This is a demo application built with the [Laravel WebSockets](https://github.com/beyondcode/laravel-websockets) package.

Be sure to check out the [official documentation](https://docs.beyondco.de/laravel-websockets/).

## Usage

1. Clone this repository
2. `composer install`
3. `cp .env.example .env`
4. `php artisan migrate`
5. `php artisan key:generate`
6. Edit config/Broadcasting.php
```
'host' => 'your.website.here',
```
7. Edit config/websockets.php
```
'local_cert' => '/etc/letsencrypt/live/your.website.here/fullchain.pem',
'local_pk' => '/etc/letsencrypt/live/your.website.here/privkey.pem',
```
8. Edit resources/js/bootstrap.js
```
window.Echo = new Echo({
    broadcaster: 'pusher',
    key: process.env.MIX_PUSHER_APP_KEY,
    wsHost: 'your.website.here',
    wssPort: 6001,
    disableStats: true,
    scheme: 'https',
    encrypted: true,
    enabledTransports: ['ws', 'wss']
});
```
9. Edit nginx config
```
server {
    listen 80;
    listen [::]:80;

    server_name your.website.here;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name your.website.here;
    root /var/www/laravel/public;

    ssl_certificate /etc/letsencrypt/live/your.website.here/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/your.website.here/privkey.pem;

    ssl_protocols TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384;
        ssl_prefer_server_ciphers on;

        add_header X-Frame-Options "SAMEORIGIN";
        add_header X-XSS-Protection "1; mode=block";
        add_header X-Content-Type-Options "nosniff";

        index index.php index.html index.htm index.nginx-debian.html;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location /ws {
        proxy_pass http://127.0.0.1:6001/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header X-Forwarded-For $remote_addr;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
    }

    location ~ /\.ht {
            deny all;
    }

    location ~ /.well-known {
            allow all;
    }
}
```
10. `php artisan websockets:serve` 
## Credits

- [Marcel Pociot](https://github.com/mpociot)
- [Freek Van der Herten](https://github.com/freekmurze)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
