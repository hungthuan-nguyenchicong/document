# corcel

> https://github.com/corcel/corcel

> composer require jgrossi/corcel

# You'll have to include CorcelServiceProvider in your config/app.php:

> Corcel\Laravel\CorcelServiceProvider::class,

> php artisan vendor:publish --provider="Corcel\Laravel\CorcelServiceProvider"

```config/database.php
// File: /config/database.php

'connections' => [

    'wordpress' => [ // for WordPress database (used by Corcel)
        'driver'    => 'mysql',
        'host'      => 'localhost',
        'database'  => 'mydatabase',
        'username'  => 'admin',
        'password'  => 'secret',
        'charset'   => 'utf8',
        'collation' => 'utf8_unicode_ci',
        'prefix'    => 'wp_',
        'strict'    => false,
        'engine'    => null,
    ],
],
```

## default

> 'connection' => 'wordpress',

```php
<?php // File: app/Post.php

namespace App;

use Corcel\Model\Post as Corcel;

class Post extends Corcel
{
    protected $connection = 'wordpress';

    public function customMethod() {
        //
    }
}
```
