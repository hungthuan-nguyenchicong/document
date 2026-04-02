<?php
namespace Ncc\Wp;

use Illuminate\Support\Facades\Blade;
use Illuminate\Support\ServiceProvider;

class WpServiceProvider extends ServiceProvider
{
    public function boot(): void
    {
        $this->loadRoutesFrom(__DIR__ . '/routesWp.php');
        $this->loadViewsFrom(__DIR__ . '/views', 'wp-view');
        Blade::anonymousComponentPath(__DIR__ . '/components', 'wp-comp');
        // 1. Phải có dòng này để định nghĩa "nhà" cho prefix wp-comp
        // Blade::componentNamespace('Ncc\\Wp\\Components', 'wp-comp');
        // Blade::componentNamespace('Vendorpath\\Wp\\Components', 'wp-compName');

    }
}
