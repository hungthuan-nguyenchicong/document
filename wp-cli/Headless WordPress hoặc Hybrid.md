## url

// Ví dụ trong Controller hoặc View của Laravel
$content = str_replace('http://localhost:8080', 'http://127.0.0.1:8080', $post->content);

> ln -s ~/git/learn-laravel/public/wp-learn/wp-content/uploads ~/git/learn-laravel/public/wp-uploads

## Cấu hình File .env của Laravel

WP_URL=http://127.0.0.1:8080
WP_API_URL=http://127.0.0.1:8080/wp-json/wp/v2

## Sử dụng WP-CLI để đồng bộ khi chuyển môi trường
> wp search-replace 'http://127.0.0.1:8080' 'https://api.yourdomain.com' --path=public/wp-learn
