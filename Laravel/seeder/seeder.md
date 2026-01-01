# php artisan
php artisan migrate:refresh --seed
## make:seeder 
php artisan make:seeder UserSeeder
## dang ky DatabaseSeeder
```bash
public function run(): void
    {
        $this->call([
            PostSeeder::class,
        ]);
        User::factory(5)->create();

        User::factory()->create([
            'name' => 'Admin User',
            'username' => 'admin',
            'password' => bcrypt('YOUR_PASSWORD'),
            'email' => 'admin@example.com',
        ]);
    }
```
## code
```bash
class PostSeeder extends Seeder
{
    /**
     * Run the database seeds.
     */
    public function run(): void
    {
        Post::factory(5)->create();
    }
}
```