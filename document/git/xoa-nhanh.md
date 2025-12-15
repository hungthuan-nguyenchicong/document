# xoa local
git branch -d feature/composer_learn
# Xóa Branch chưa Merge (Buộc xóa)
phải dùng cờ -D (viết tắt của --delete --force).

git branch -D <tên_nhánh>

# Xóa Branch trên Remote (Server)

git push <tên_remote> --delete <tên_nhánh>

git push origin --delete feature/composer