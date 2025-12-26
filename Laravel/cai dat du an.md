# tao ngay tai goc
## xoa tat ca
rm -rf ./* ./.* 2>/dev/null
## tao du an
composer create-project laravel/laravel .
# thu muc con
laravel new example-app

cd example-app

npm install && npm run build

composer run dev
