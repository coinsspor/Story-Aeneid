# 🌌 Story Protocol - Aeneid Node Installer

Welcome to the automatic installer for the **Story Protocol Aeneid Network**!  
This guide helps you install, configure, start, and monitor a full node — all in just a few minutes.

---

## 🧩 Features

✅ Automatic installation of dependencies  
✅ Compiles `story-geth` (execution client)  
✅ Installs the `story` consensus binary  
✅ Dynamically sets ports via prefix  
✅ Generates systemd services  
✅ Provides a live sync tracker  

---

## 🚀 One-Line Installation

```bash
bash <(curl -s https://raw.githubusercontent.com/coinsspor/Story-Aeneid/refs/heads/main/Story-Aeneid.sh)
```

You will be prompted for:

Your Node Moniker (name)

Your Port Prefix (e.g. 56 → 56657, 56656, etc.)

## 🔌 Port Structure
Service	Port Formula	Example (prefix=56)

RPC	${prefix}657	56657

P2P	${prefix}656	56656

Prometheus	${prefix}660	56660

API Address	${prefix}317	56317

Geth Auth Engine	${prefix}551	56551

Geth HTTP	${prefix}854	56854

Geth WebSocket	${prefix}855	56855


## ⚙️ Load & Start Services
### 🔧 Start story-geth
```bash
sudo systemctl daemon-reload && \
sudo systemctl start story-geth && \
sudo systemctl enable story-geth && \
sudo systemctl status story-geth
```
### 🔧 Start story 
```bash
sudo systemctl daemon-reload && \
sudo systemctl start story && \
sudo systemctl enable story && \
sudo systemctl status story
```

## 📜 View Logs
### 🔍 story-geth Logs
```bash
sudo journalctl -u story-geth -f -o cat
```
### 🔍 story Logs
```bash
sudo journalctl -u story -f -o cat
```

## 📡 Check Node Sync Status
```bash
curl localhost:<your_rpc_port>/status | jq
```
## ✨ Create Validator
Create/Register validator:
```bash
story validator create --stake 1024000000000000000000 --moniker "your-node-name" --chain-id 1315 --unlocked=false --private-key "your-evm-priv-key"
```

## 🧭 Live Block Sync Monitor
Use the following script to track how many blocks are left to reach the network head:
```bash
#!/bin/bash

# Get local RPC port from config
rpc_port=$(grep -m 1 -oP '^laddr = "\K[^"]+' "$HOME/.story/story/config/config.toml" | cut -d ':' -f 3)

local_rpc="localhost:$rpc_port"
network_rpc="https://aeneid.storyrpc.io"

while true; do
  local_height=$(curl -s "$local_rpc/status" | jq -r '.result.sync_info.latest_block_height')
  network_height=$(curl -s "$network_rpc/status" | jq -r '.result.sync_info.latest_block_height')

  if ! [[ "$local_height" =~ ^[0-9]+$ ]] || ! [[ "$network_height" =~ ^[0-9]+$ ]]; then
    echo -e "\033[1;31mError: Failed to fetch block heights. Retrying...\033[0m"
    sleep 5
    continue
  fi

  blocks_left=$((network_height - local_height))
  if [ "$blocks_left" -lt 0 ]; then
    blocks_left=0
  fi

  echo -e "\033[1;33mNode Height:\033[1;34m $local_height\033[0m \033[1;33m| Network Height:\033[1;36m $network_height\033[0m \033[1;33m| Blocks Left:\033[1;31m $blocks_left\033[0m"
  sleep 5
done

```






