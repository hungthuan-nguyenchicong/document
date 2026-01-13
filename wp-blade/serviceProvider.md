# php

```php
<?php

namespace Wp;

use Illuminate\Support\ServiceProvider;

class WpServiceProvider extends ServiceProvider
{
    public function boot(): void
    {
        $this->loadRoutesFrom(__DIR__.'/wp-routes.php');
        $this->loadViewsFrom(__DIR__.'/Posts/blade', 'wp-post');
    }
}

```
