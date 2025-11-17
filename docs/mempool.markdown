---
title: Mempool
nav_order: 6
---

<img title="mempool" alt="mempool" src="https://raw.githubusercontent.com/mempool/mempool/refs/heads/master/frontend/src/resources/mempool-space-preview.png" height="50%" width="50%">

<br />

```
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
```
```
sudo apt install mariadb-server mariadb-client
```
```
curl https://gist.githubusercontent.com/wiz/8f585a0eb4e3886ba574e3072f6b1e2f/raw/7708491cfdfbbf0f7a17cbdfdb9d27cc80438caf/913C5FF1F579B66CA10378DBA394E332255A6173.asc | gpg --import
```
```
git clone https://github.com/mempool/mempool.git && \
cd mempool
```
```
git checkout v3.2.1 && \
git verify-tag v3.2.1
```
```
sudo mysql
```
```
create database mempool;
```
```
grant all privileges on mempool.* to 'mempool'@'localhost' identified by 'mempool';
```
```
exit
```
```
cd backend && \
npm install --prod && \
npm run build
```
```
cd ../frontend/ && \
npm install --prod && \
npm run build
```
```
echo "
[Unit]
Description=Mempool daemon
Wants=bitcoind.service
After=mariadb.service bitcoind.service

[Service]
WorkingDirectory=/home/mempool/mempool/backend
ExecStart=/usr/bin/node --max-old-space-size=2048 dist/index.js

User=mempool
Restart=on-failure
RestartSec=600

# Hardening measures
PrivateTmp=true
ProtectSystem=full
NoNewPrivileges=true
PrivateDevices=true

[Install]
WantedBy=multi-user.target" | sudo tee /etc/systemd/system/mempool.service
```