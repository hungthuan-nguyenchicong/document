# openlitespeed
## cai dat
```bash
sudo wget -O - https://repo.litespeed.sh | sudo bash
```
### cai dat
```bash
sudo apt-get -y install openlitespeed
```
## kiem tra
```bash
/usr/local/lsws/bin/openlitespeed -v
```
## chay
``` bash
sudo systemctl start lsws

sudo systemctl restart lsws
```
## xem cong
```bash
ss -tuln | grep -E '8088|7080'
```

## Reset the WebAdmin password
```bash
sudo /usr/local/lsws/admin/misc/admpass.sh
```
### Check OpenLiteSpeed version
```bash
/usr/local/lsws/bin/lshttpd -v
```
### Check for OpenLiteSpeed configuration errors
```bash
sudo /usr/local/lsws/bin/openlitespeed -t
```
## thay dôi http 7080
```bash
sudo cat /usr/local/lsws/admin/conf/admin_config.conf
```
secure   1 -> https 0 -> http

```bash
sudo systemctl restart lsws
```

http://localhost:8088

http://localhost:7080