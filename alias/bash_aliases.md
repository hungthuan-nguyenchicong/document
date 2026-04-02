nano ~/.bash_aliases

# Thêm dòng này vào ~/.bash_aliases

reload-bash() {
source ~/.bashrc
echo "--- Da cap nhat code moi cho Bash! ---"
}

# Khai bao biến

dirApp=~/git/learn-docker

# cd app

cd-app() {
cd "$dirApp"
}

# nano ~/.bashrc

nano ~/.bashrc

# Neu file .bash_aliases ton tai, hay nap no

if [ -f ~/.bash_aliases ]; then
. ~/.bash_aliases
fi

source ~/.bashrc
