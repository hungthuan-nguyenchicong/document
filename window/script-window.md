# wsl-laravel-learn-artisan.bat
```bash
@echo off
powershell -command "Start-Process wsl -ArgumentList '-e bash -i -c \"cd ~/git/learn-laravel; php artisan; exec bash\"'
```
# cmd-learn-laravel.bat
```bash
@echo off
powershell -WindowStyle Minimized -NoExit -Command "wsl -e bash -i -c 'cd ~/git/learn-laravel; code learn-laravel.code-workspace ;composer run dev; exec bash'"
```