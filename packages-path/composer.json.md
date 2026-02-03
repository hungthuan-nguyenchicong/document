# composer.json

```composer.json
{
  "name": "vendor-path/packages-path",
  "description": "Một package siêu cấp PATH",
  "autoload": {
    "psr-4": {
      "Vendorpath\\Src\\": "src/",
      "Vendorpath\\Wp\\": "wp/"
    }
  }
}
```

## laravel/composer.json

```composer.json
    "repositories": [
        {
            "type": "path",
            "url": "../packages-path",
            "options": {
                "symlink": true
            }
        }
    ],
    "require": {
        "vendor-path/packages-path": "dev-*"
    },
```

## run

> composer update vendor-path/packages-path

> composer dump-autoload
