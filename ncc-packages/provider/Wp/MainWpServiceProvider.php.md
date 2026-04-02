<?php

namespace Ncc\Wp;

use Illuminate\Support\AggregateServiceProvider;

class MainWpServiceProvider extends AggregateServiceProvider {
    protected $providers = [
        WpServiceProvider::class,
        \Corcel\Laravel\CorcelServiceProvider::class,
    ];
}
