---
title: BTC-RPC-Explorer
nav_order: 5
---
# BTC-RPC-Explorer
<br />
```
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
```
```
sudo apt update && sudo apt install nodejs
```
```
curl https://keybase.io/danjanosik/pgp_keys.asc | gpg --import
```
```
git clone https://github.com/janoside/btc-rpc-explorer.git && \
cd btc-rpc-explorer
```
```
git checkout v3.5.1 && \
git verify-commit 8ed77ab
```
```
npm install
```
```
echo "
[Unit]
Description=btc-rpc-explorer
Wants=bitcoind.service
After=bitcoind.service

[Service]
WorkingDirectory=/home/btcrpcexplorer/btc-rpc-explorer
ExecStart=/usr/bin/npm start
Restart=on-failure
RestartSec=20

User=btcrpcexplorer

# Hardening measures
PrivateTmp=true
ProtectSystem=full
NoNewPrivileges=true
PrivateDevices=true

[Install]
WantedBy=multi-user.target" | sudo tee /etc/systemd/system/btcrpcexplorer.service
```