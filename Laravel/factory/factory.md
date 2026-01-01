# php artisan
## make:facktory
php artisan make:factory PostFactory
## code
```bash
public function definition(): array
    {
        return [
            //
            'title' => fake()->sentence(),
            'content' => fake()->paragraphs(5, true),
        ];
    }
```