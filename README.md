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
9. `php artisan websockets:serve` 
## Credits

- [Marcel Pociot](https://github.com/mpociot)
- [Freek Van der Herten](https://github.com/freekmurze)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
