# Upgrade Your 0G Storage Node to 1.1.0👇
---
## For BOTH VPS & Local PC
---
### Stop your Node
```
sudo systemctl stop zgs
```

### Change Directory and Upgrade to the latest Version
```
cd ~/0g-storage-node
```

```
git reset --hard
git clean -fd
```

```
git fetch --all
git checkout v1.1.0
```

```
git submodule update --init --recursive
```

### Build Latest Cargo-Release
```
sudo apt-get install protobuf-compiler
```

```
cargo build --release
```
---

## Now Set Your Config. toml file
```
rm -rf $HOME/0g-storage-node/run/config.toml
```

```
curl -o $HOME/0g-storage-node/run/config.toml https://raw.githubusercontent.com/imysryasir/0G-Full-Update-Guide--with-Video-tutorial/refs/heads/main/config.toml

```

### Now Open config to add your Miner key(metamask key) 
```
nano $HOME/0g-storage-node/run/config.toml
```
---
## Now Create Service File
```
sudo tee /etc/systemd/system/zgs.service > /dev/null <<EOF
[Unit]
Description=ZGS Node
After=network.target

[Service]
User=$USER
WorkingDirectory=$HOME/0g-storage-node/run
ExecStart=$HOME/0g-storage-node/target/release/zgs_node --config $HOME/0g-storage-node/run/config.toml
Restart=on-failure
RestartSec=10
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

# Start The Node and you are good to go

```
sudo systemctl daemon-reload
```
```
sudo systemctl enable zgs
```
```
sudo systemctl start zgs
```

### Check Logs and Block
```
tail -f ~/0g-storage-node/run/log/zgs.log.$(TZ=UTC date +%Y-%m-%d)
```
```
 while true; do     response=$(curl -s -X POST http://localhost:5678 -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"zgs_getStatus","params":[],"id":1}');     logSyncHeight=$(echo $response | jq '.result.logSyncHeight');     connectedPeers=$(echo $response | jq '.result.connectedPeers');     echo -e "logSyncHeight: \033[32m$logSyncHeight\033[0m, connectedPeers: \033[34m$connectedPeers\033[0m";     sleep 5; done
```

## Restart Node
```
sudo systemctl restart zgs
```

# 📬 Subscribe: https://youtube.com/@KindCrypto
# 📢 TG Alpha Drops: https://t.me/kind_cr
# 📍 Twitter: https://x.com/armaanbhat201
