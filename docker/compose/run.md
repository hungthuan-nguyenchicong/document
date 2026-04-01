> docker compose ps

> docker compose exec -it php-handler sh

> exit

> docker volume ls

> docker volume inspect learn-docker_db_storage

# 1. Dừng và xóa container, đồng thời xóa luôn volume liên quan (-v)

docker compose down -v

# 1. Dừng container trước

docker compose stop

# 2. Xóa volume bằng tên

docker volume rm learn-docker_db_storage
