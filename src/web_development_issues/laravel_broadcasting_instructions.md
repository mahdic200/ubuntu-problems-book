# laravel broadcasting instructions

first of all configure laravel workers, the easiest way is to use `database` driver .

```bash
php artisan queue:table

php artisan migrate
```

then go to the `.env` file and change the `QUEUE_CONNECTION=`
 line to `QUEUE_CONNECTION=database`

when you want to make a job to be done , you have to set a worker . you can read  about this in documentation but first learn this command :

```bash
php artisan queue:work
```

if you have priority you can set priority like this :

```bash
php artisan queue:work --queue=high,default
```

in this example , the `high` queue has more importance than `default` queue .

you have to use `supervisor` for manging the above command , cause there is a probability that it might fail and your workers won't work anymore .

so the next thing is to install `supervisor` (debian) :

```bash
sudo apt install supervisor
```

now we have to configure it :

```bash
sudo nano /etc/supervisor/conf.d/laravel-worker.conf
```

and the content might be :

```conf
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /home/forge/app.com/artisan queue:work sqs --sleep=3 --tries=3 --max-time=3600
autostart=true
autorestart=true
stopasgroup=true
killasgroup=true
user=forge
numprocs=8
redirect_stderr=true
stdout_logfile=/home/forge/app.com/worker.log
stopwaitsecs=3600
```

you have to change the `forge` user and command according to your user and project page .

activating :

```bash
sudo supervisorctl reread

sudo supervisorctl reread

sudo supervisorctl start "laravel-worker:*"
```

you have to apply broadcasting for your event :

```php
<?php
 
namespace App\Events;
 
use App\Models\Order;
use Illuminate\Broadcasting\Channel;
use Illuminate\Broadcasting\InteractsWithSockets;
use Illuminate\Broadcasting\PresenceChannel;
use Illuminate\Contracts\Broadcasting\ShouldBroadcast;
use Illuminate\Queue\SerializesModels;
 
class OrderShipmentStatusUpdated implements ShouldBroadcast
{
    /**
     * The order instance.
     *
     * @var \App\Models\Order
     */
    public $order;
}
```

and this is initially queued , so you needed the worker part mentioned above .

and the last thing , you have to use `soketi` websocket server and `pusher` driver for your laravel application .

first of all , go to `api.php` top of all routes write this :

```php
<?php

use Illuminate\Support\Facades\Broadcast;

Broadcast::routes(['middleware' => ['auth:sanctum']]);
```

I assumed you are using sanctum as your api authentication package .

and then :

```bash
composer require pusher/pusher-php-server
```

and change the `BROADCAST_DRIVER=` to `BROADCAST_DRIVER=pusher` in `.env` file .

you have to change the `broadcasting.php` file too :

```php
'connections' => [

	// snip

	'pusher' => [
		'driver' => 'pusher',
		'key' => env('PUSHER_APP_KEY'),
		'secret' => env('PUSHER_APP_SECRET'),
		'app_id' => env('PUSHER_APP_ID'),
		'options' => [
			'cluster' => env('PUSHER_APP_CLUSTER'),
			'host' => env('PUSHER_HOST') ?: 'api-'.env('PUSHER_APP_CLUSTER', 'mt1').'.pusher.com',
			'port' => env('PUSHER_PORT', 443),
			'scheme' => env('PUSHER_SCHEME', 'https'),
			'encrypted' => false,
			'useTLS' => env('PUSHER_SCHEME', 'https') === 'https',
		],
	],

	// snip

],
```


and in your server :

```bash
$ npm --version
v18.20.6
```

you must have `nodejs` version 14 or 16 or 18 LTS .

and you may start it via `supervisor` . make a configuration file like `/etc/supervisor/conf.d/soketi.conf` and a config file for soketi `/etc/soketi/config.json`:

```bash
[program:soketi]
process_name=%(program_name)s_%(process_num)02d
command=soketi start --config=/path/to/config.json
autostart=true
autorestart=true
stopasgroup=true
killasgroup=true
user=mahdi
numprocs=1
redirect_stderr=true
stdout_logfile=/var/log/soketi-supervisor.log
stopwaitsecs=60
stopsignal=sigint
minfds=10240
```

and contents of the `config.json` file :

```json
{
    "debug": true,
    "port": 6001,
    "appManager.array.apps": [
        {
            "id": "id",
            "key": "api-key",
            "secret": "secret",
            "webhooks": [
                {
                    "url": "http://127.0.0.1",
                    "event_types": ["channel_occupied"]
                }
            ]
        }
    ]
}
```

you can generate the `id`, `api-key` and `secret` with `openssl` library in Linux or manually pressing your keyboard . but let's show you how you can do it :
below code produces a random string with `32` characters in `base64` :

```bash
$ openssl rand -base64 24
d0wU1tBWSwdApGSkR/A3VdtPHGv6xSTTZKB1zCKqS24=
```

notice that the above code produces 8 more characters than it's input argument which in this example is `24` so the output length will be a string with `24 + 8 = 32` characters . 

done , good luck :) !


