---
title: LND
nav_order: 7
---

<img title="LND" alt="LND" src="https://raw.githubusercontent.com/lightningnetwork/lnd/refs/heads/master/logo.png" height="25%" width="25%">

<br />

```
sudo apt install git make gcc curl gnupg ca-certificates jq bash
```
```
wget https://go.dev/dl/go1.25.4.linux-amd64.tar.gz && \
echo "9fa5ffeda4170de60f67f3aa0f824e426421ba724c21e133c1e35d6159ca1bec  go1.25.4.linux-amd64.tar.gz" | sha256sum --check
```
```
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.25.4.linux-amd64.tar.gz && \
export PATH=$PATH:/usr/local/go/bin && \
```
```
curl https://raw.githubusercontent.com/lightningnetwork/lnd/master/scripts/keys/roasbeef.asc | gpg --import
```
```
git clone https://https://github.com/lightningnetwork/lnd.git && \
cd lnd
```
```
git checkout v0.20.0-beta && \
git verify-tag v0.20.0-beta
```
```
make clean && make release-install
```
```
sudo install -m 0755 -o root -g root -t /usr/local/bin/ ~/go/bin/ln{d,cli}
```
```
sudo cp contrib/lncli.bash-completion /etc/bash_completion.d/
```
```
echo "
[Unit]
Description=Lightning Network Daemon
Requires=bitcoind.service
After=bitcoind.service

[Service]
ExecStart=/usr/local/bin/lnd
ExecStop=/usr/local/bin/lncli stop

User=lnd
Group=lnd

Restart=on-failure
RestartSec=60
Type=notify

TimeoutStartSec=1200
TimeoutStopSec=3600

# Hardening Measures
ProtectSystem=full
NoNewPrivileges=true
PrivateDevices=true
MemoryDenyWriteExecute=true

[Install]
WantedBy=multi-user.target" | sudo tee /etc/systemd/system/lnd.service
```