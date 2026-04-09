apk add libstdc++

code

sudo apk add git

sudo apk add git openssh

ssh-keygen -t ed25519 -C "email@gmail.com"

# Khởi động agent
eval $(ssh-agent -s)

# Thêm key của bạn vào agent
ssh-add ~/.ssh/id_ed25519

sudo vi /etc/profile

# Tự động chạy SSH Agent
if [ -z "$SSH_AUTH_SOCK" ]; then
    eval $(ssh-agent -s) > /dev/null
    ssh-add ~/.ssh/id_ed25519 2>/dev/null
fi

cat ~/.ssh/id_ed25519.pub

ssh -T git@github.com

git clone git@github.com:hungthuan-nguyenchicong/wsl-alpine.git

git config --global user.email "email@gmail.com"
git config --global user.name "Nguyen Chi Cong"