---
title: Electrs
nav_order: 4
---

<img title="electrs" alt="electrs" src="https://raw.githubusercontent.com/romanz/electrs/refs/heads/master/logo/logo.svg" height="50%" width="50%">

<br />

```
sudo apt install clang cmake
```
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
```
source "$HOME/.cargo/env"
```
```
cargo --version
```

{: .info }
cargo 1.91.1 (ea2d97820 2025-10-10)

```
mkdir -p ~/.config/git && \
touch ~/.config/git/allowed_signers
```
```
echo "git@romanzey.de ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAZVq/3fgkildjN/MqEnhrP5550sDpFzGxMwevr5q/9w" > ~/.config/git/allowed_signers
(see https://github.com/romanz/keys)
```
```
git config gpg.ssh.allowedSignersFile "$HOME/.config/git/allowed_signers"
```
```
git clone https://github.com/romanz/electrs.git && \
cd electrs
```
```
git checkout v0.11.1 && \
git log --show-signature
```

{: .info }
commit 35216c6d30148be8e6763d913d437330f431fc03 (HEAD, tag: v0.11.1)
Good "git" signature for git@romanzey.de with ED25519 key SHA256:GifMn7F2swVKyn6MewbQHrYCs4i/bPK7gnwxhuPz/YA)

```
cargo build --locked --release
```
```
sudo install -m 0755 -o root -g root -t /usr/local/bin/ /home/electrs/electrs/target/release/electrs
```
```
echo "
[Unit]
Description=Electrs
After=bitcoind.service

[Service]
WorkingDirectory=/home/electrs/electrs
ExecStart=/usr/local/bin/electrs
User=electrs
Group=electrs
Type=simple
TimeoutSec=60
Restart=always
RestartSec=60

# Hardening measures
PrivateTmp=true
ProtectSystem=full
NoNewPrivileges=true
PrivateDevices=true

[Install]
WantedBy=multi-user.target" | sudo tee /etc/systemd/system/electrs.service
```
