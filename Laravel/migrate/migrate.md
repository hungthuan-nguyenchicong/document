# php artisan
## dev
php artisan migrate:refresh --seed
## code
```bash
public function up(): void
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->longText('content');
            $table->timestamps();
        });
    }

```
