# 🌌 Story Protocol Aeneid Testnet Complete Node Setup Guide

> **The Ultimate Guide to Setting Up Your Story Protocol Aeneid Testnet Node with Cosmovisor**

Welcome to the most comprehensive guide for setting up a Story Protocol Aeneid Testnet node! This guide covers everything from installation to validator setup, monitoring, and maintenance.

---

## 📋 Table of Contents

- [📡 Endpoints & Resources](#endpoints--resources)
- [🏅 Competition Resources](#competition-resources)
- [🖥️ System Requirements](#system-requirements)
- [🚀 Installation](#installation)
- [🔄 Cosmovisor Setup](#cosmovisor-setup)
- [⚙️ Configuration](#configuration)
- [🔧 Create System Services](#create-system-services)
- [⚡ Snapshot Service](#snapshot-service)
- [🏁 Start Services](#start-services)
- [📊 Monitoring & Sync Status](#monitoring--sync-status)
- [🏆 Validator Creation](#validator-creation)
- [🔧 Useful Commands](#useful-commands-1)
- [🔄 Upgrades & Maintenance](#upgrades--maintenance)
- [🔍 Troubleshooting](#troubleshooting)

---

## 📡 Endpoints & Resources

### 🌟 Coinsspor Endpoints

| Service | Endpoint | Type |
|---------|----------|------|
| 🔹 **Cosmos RPC** | [https://story-aeneid-testnet-rpc.coinsspor.com](https://story-aeneid-testnet-rpc.coinsspor.com) | REST API |
| 🔹 **Cosmos WebSocket** | [wss://story-aeneid-testnet-rpc.coinsspor.com/websocket](wss://story-aeneid-testnet-rpc.coinsspor.com/websocket) | WebSocket |
| 🔹 **Cosmos REST API** | [https://story-aeneid-testnet-api.coinsspor.com](https://story-aeneid-testnet-api.coinsspor.com) | REST API |
| 🟣 **EVM RPC** | [https://story-aeneid-testnet-evm.coinsspor.com](https://story-aeneid-testnet-evm.coinsspor.com) | JSON-RPC |
| 🟣 **EVM WebSocket** | [wss://story-aeneid-testnet-evm.coinsspor.com](wss://story-aeneid-testnet-evm.coinsspor.com) | WebSocket |

### 🌐 Official Resources

| Resource | Link | Description |
|----------|------|-------------|
| **Chain ID** | `1315` | Aeneid Testnet Chain ID |
| **Official RPC** | [https://aeneid.storyrpc.io](https://aeneid.storyrpc.io) | Official RPC Endpoint |
| **Explorer** | [https://aeneid.storyscan.io](https://aeneid.storyscan.io) | Block Explorer |
| **IP Explorer** | [https://aeneid.explorer.story.foundation](https://aeneid.explorer.story.foundation) | IP Explorer |
| **Faucet** | [https://faucet.story.foundation](https://faucet.story.foundation) | Testnet Faucet |

### 🛠️ Development Resources

| Resource | Link | Description |
|----------|------|-------------|
| **Story Geth Releases** | [https://github.com/piplabs/story-geth/releases](https://github.com/piplabs/story-geth/releases) | Latest Releases |
| **Story Releases** | [https://github.com/piplabs/story/releases](https://github.com/piplabs/story/releases) | Latest Releases |
| **Documentation** | [https://docs.story.foundation](https://docs.story.foundation) | Official Docs |
| **GitHub** | [https://github.com/piplabs](https://github.com/piplabs) | Source Code |

---

## 🏅 Competition Resources

**🏆 Story Protocol Validator Competition**

| Resource | Link | Description |
|----------|------|-------------|
| **📊 Round 1 Results** | [https://storytable.coinsspor.com](https://storytable.coinsspor.com) | Complete Round 1 Stats |
| **📊 Round 2 Tracking** | [https://storytable.coinsspor.com/round2](https://storytable.coinsspor.com/round2) | Current Round Stats |

**📅 Competition Period:** June 30, 2025 - July 28, 2025  
**🎯 Active Validators:** 80  
**📈 Average Uptime:** 99.9%

---

## 🖥️ System Requirements

### Minimum Hardware Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **CPU** | 4 cores | 8 cores |
| **RAM** | 16 GB | 32 GB |
| **Storage** | 500 GB NVMe SSD | 1 TB NVMe SSD |
| **OS** | Ubuntu 22.04 LTS | Ubuntu 22.04 LTS |
| **Network** | 100 Mbps | 1 Gbps |

### 🔌 Port Configuration (Dynamic based on your prefix)

| Service | Port Formula | Example (prefix=26) | Example (prefix=56) |
|---------|--------------|---------------------|---------------------|
| Story P2P | `${STORY_PORT}656` | 26656 | 56656 |
| Story RPC | `${STORY_PORT}657` | 26657 | 56657 |
| Story API | `${STORY_PORT}317` | 26317 | 56317 |
| Story Prometheus | `${STORY_PORT}660` | 26660 | 56660 |
| Story-Geth P2P | `${STORY_PORT}303` | 26303 | 56303 |
| Story-Geth RPC | `${STORY_PORT}545` | 26545 | 56545 |
| Story-Geth WS | `${STORY_PORT}546` | 26546 | 56546 |
| Story-Geth Auth | `${STORY_PORT}551` | 26551 | 56551 |
| Story-Geth Metrics | `${STORY_PORT}060` | 26060 | 56060 |

---

## 🚀 Installation

### Step 1: System Preparation

```bash
# Update system packages
sudo apt update && sudo apt upgrade -y

# Install dependencies
sudo apt install -y curl git wget htop tmux build-essential jq make lz4 gcc unzip aria2
```

### Step 2: Install Go 1.22.11

```bash
cd $HOME
VER="1.22.11"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"

# Add to PATH
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source ~/.bash_profile

# Verify installation
go version
```

### Step 3: Set Variables and Port Configuration

```bash
# Set your moniker (node name)
echo "export STORY_MONIKER=\"YourNodeName\"" >> $HOME/.bash_profile

# Set port prefix (change this to your desired prefix, e.g., 26, 56, 62, etc.)
echo "export STORY_PORT=\"26\"" >> $HOME/.bash_profile

source $HOME/.bash_profile

# Verify variables
echo "Moniker: $STORY_MONIKER"
echo "Port Prefix: $STORY_PORT"
```

### Step 4: Install Story-Geth (Execution Client)

```bash
cd $HOME
rm -rf story-geth
git clone https://github.com/piplabs/story-geth.git
cd story-geth
git checkout v1.1.1
go build -v ./cmd/geth
mv ./geth $HOME/go/bin/story-geth

# Verify installation
story-geth version
```

### Step 5: Install Story (Consensus Client)

```bash
cd $HOME
rm -rf story
git clone https://github.com/piplabs/story
cd story
git checkout v1.3.0
go build -o story ./client 
mv ./story $HOME/go/bin/story

# Verify installation
story version
```

### Step 6: Initialize Node

```bash
# Initialize the node
story init --moniker $STORY_MONIKER --network aeneid

# Create necessary directories
mkdir -p $HOME/.story/story
mkdir -p $HOME/.story/geth
```

---

## 🔄 Cosmovisor Setup

### Step 1: Install Cosmovisor

```bash
# Install Cosmovisor
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.6.0

# Verify installation
cosmovisor version
```

### Step 2: Configure Cosmovisor Environment

```bash
# Set environment variables
echo "export DAEMON_NAME=story" >> $HOME/.bash_profile
echo "export DAEMON_HOME=$HOME/.story/story" >> $HOME/.bash_profile
echo "export DAEMON_DATA_BACKUP_DIR=$DAEMON_HOME/cosmovisor/backup" >> $HOME/.bash_profile
echo "export DAEMON_ALLOW_DOWNLOAD_BINARIES=false" >> $HOME/.bash_profile
echo "export DAEMON_RESTART_AFTER_UPGRADE=true" >> $HOME/.bash_profile

source $HOME/.bash_profile
```

### Step 3: Initialize Cosmovisor with Current Binary

```bash
# Initialize cosmovisor with story binary
cosmovisor init $(which story)

# Create backup directory
mkdir -p $DAEMON_HOME/cosmovisor/backup

# Set proper permissions
sudo chown -R $USER:$USER $HOME/.story
```

---

## ⚙️ Configuration

### Step 1: Configure Ports

```bash
# Configure Story ports using your prefix
sed -i.bak -e "s%:1317%:${STORY_PORT}317%g;
s%:8551%:${STORY_PORT}551%g" $HOME/.story/story/config/story.toml

sed -i.bak -e "s%:26658%:${STORY_PORT}658%g;
s%:26657%:${STORY_PORT}657%g;
s%:26656%:${STORY_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${STORY_PORT}656\"%;
s%:26660%:${STORY_PORT}660%g" $HOME/.story/story/config/config.toml

echo "✅ Configured ports with prefix: $STORY_PORT"
echo "🌐 RPC Port: ${STORY_PORT}657"
echo "🔗 P2P Port: ${STORY_PORT}656"
echo "📡 API Port: ${STORY_PORT}317"
```

### Step 2: Configure Seeds and Peers

```bash
SEEDS="944e8889ecd7c13623ef1081aae4555d6f525041@b1-b.odyssey-devnet.storyrpc.io:26656"
PEERS="3b1aaa03f996d619cb2f4230ebace45686ab3b8a@34.140.167.127:26656,36ca8b119bf5851cd1e37060af914cb07dec24f9@34.79.40.193:26656,2a28bd1a6ecb0a1d8ceade599b311d202447d635@193.122.141.78:26656"

sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*seeds *=.*/seeds = \"$SEEDS\"/}" \
       -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.story/story/config/config.toml
```

### 🍀 Fresh Peers Discovery (Recommended)

For better connectivity and faster sync, use our fresh peer discovery system:

```bash
# Get fresh peers from Coinsspor RPC
URL="https://story-aeneid-testnet-rpc.coinsspor.com/net_info"

echo "🔍 Discovering fresh peers from Coinsspor RPC..."
response=$(curl -s $URL)

# Extract active peers with valid IP addresses
PEERS=$(echo $response | jq -r '.result.peers[] | select(.remote_ip | test("^[0-9]{1,3}(\\.[0-9]{1,3}){3}$")) | "\(.node_info.id)@\(.remote_ip):" + (.node_info.listen_addr | capture(":(?<port>[0-9]+)$").port)' | paste -sd "," -)

if [ -n "$PEERS" ]; then
    echo "✅ Found fresh peers: $PEERS"
    echo "🔧 Updating config.toml with fresh peers..."
    
    # Update persistent_peers in config.toml
    sed -i 's|^persistent_peers *=.*|persistent_peers = "'$PEERS'"|' $HOME/.story/story/config/config.toml
    
    echo "✅ Fresh peers configured successfully!"
else
    echo "⚠️  No fresh peers found, using default peers"
fi
```

### Step 3: Configure Indexer and Prometheus

```bash
# Enable Prometheus and disable indexer for better performance
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.story/story/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.story/story/config/config.toml
```

### Step 4: Download Genesis and Addrbook

```bash
# Download genesis file from Coinsspor GitHub
wget -O $HOME/.story/story/config/genesis.json https://raw.githubusercontent.com/coinsspor/Story-Aeneid/refs/heads/main/genesis.json

# Download addrbook for faster peer discovery
wget -O $HOME/.story/story/config/addrbook.json https://raw.githubusercontent.com/coinsspor/Story-Aeneid/refs/heads/main/addrbook.json

# Verify files are downloaded correctly
echo "✅ Verifying downloaded files..."

if [ -f "$HOME/.story/story/config/genesis.json" ]; then
    genesis_size=$(stat -c%s "$HOME/.story/story/config/genesis.json")
    echo "✅ Genesis file downloaded successfully (Size: $genesis_size bytes)"
else
    echo "❌ Genesis download failed"
    exit 1
fi

if [ -f "$HOME/.story/story/config/addrbook.json" ]; then
    addrbook_size=$(stat -c%s "$HOME/.story/story/config/addrbook.json")
    echo "✅ Addrbook downloaded successfully (Size: $addrbook_size bytes)"
else
    echo "⚠️  Addrbook download failed - node will discover peers automatically"
fi

# Set proper permissions
chmod 644 $HOME/.story/story/config/genesis.json
chmod 644 $HOME/.story/story/config/addrbook.json

echo "✅ Genesis and addrbook setup completed"
```

---

## 🔧 Create System Services

### Story-Geth Service

```bash
sudo tee /etc/systemd/system/story-geth.service > /dev/null <<EOF
[Unit]
Description=Story Geth Client
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/story-geth --aeneid --syncmode full --port ${STORY_PORT}303 --http --http.api eth,net,web3,engine --http.vhosts '*' --http.addr 0.0.0.0 --http.port ${STORY_PORT}545 --authrpc.addr 127.0.0.1 --authrpc.port ${STORY_PORT}551 --authrpc.vhosts=* --ws --ws.api eth,web3,net,txpool --ws.addr 0.0.0.0 --ws.port ${STORY_PORT}546 --ws.origins '*' --metrics --metrics.addr 0.0.0.0 --metrics.port ${STORY_PORT}060
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Story Service with Cosmovisor

```bash
sudo tee /etc/systemd/system/story.service > /dev/null <<EOF
[Unit]
Description=Story Cosmovisor
After=network.target

[Service]
Type=simple
User=$USER
ExecStart=$HOME/go/bin/cosmovisor run run --api-enable --api-address=0.0.0.0:${STORY_PORT}317
Restart=on-failure
RestartSec=5
LimitNOFILE=65535
Environment="DAEMON_NAME=story"
Environment="DAEMON_HOME=$HOME/.story/story"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_DATA_BACKUP_DIR=$HOME/.story/story/cosmovisor/backup"
WorkingDirectory=$HOME/.story/story

[Install]
WantedBy=multi-user.target
EOF
```

---

## ⚡ Snapshot Service

### 📸 Coinsspor Snapshot Service

**High-performance snapshot service for fast node synchronization**

- 🔄 **Updated**: Every 6 hours (00:00, 06:00, 12:00, 18:00 UTC)
- 🗜️ **Type**: Pruned snapshots (indexer: null)
- 📦 **Compression**: LZ4 format for optimal speed
- 🔒 **Security**: SSL secured with validator-safe backup/restore
- 💾 **Size**: ~40GB Cosmos + ~16GB Geth (vs ~200GB+ full node)

#### Check Latest Snapshot Height
```bash
echo "Coinsspor Snapshot Height: $(curl -s https://snaps.coinsspor.com/story/aeneid/block-height.txt)"
```

#### Download and Apply Snapshot
```bash
# Stop services
sudo systemctl stop story story-geth

# Backup validator state (CRITICAL for validators)
mv $HOME/.story/story/data/priv_validator_state.json $HOME/.story/priv_validator_state.json.backup

# Clean data directories
rm -rf $HOME/.story/story/data
rm -rf $HOME/.story/geth/aeneid/geth/chaindata
mkdir -p $HOME/.story/geth/aeneid/geth

# Auto-detect and download latest snapshots
SNAPSHOT_URL="https://snaps.coinsspor.com/story/aeneid/"
LATEST_COSMOS=$(curl -s $SNAPSHOT_URL | grep -oP 'story_\d{8}-\d{4}_\d+_cosmos\.tar\.lz4' | sort | tail -n 1)
LATEST_GETH=$(curl -s $SNAPSHOT_URL | grep -oP 'story_\d{8}-\d{4}_\d+_geth\.tar\.lz4' | sort | tail -n 1)

if [ -n "$LATEST_COSMOS" ] && [ -n "$LATEST_GETH" ]; then
  COSMOS_URL="${SNAPSHOT_URL}${LATEST_COSMOS}"
  GETH_URL="${SNAPSHOT_URL}${LATEST_GETH}"

  # Verify URLs are accessible
  if curl -s --head "$COSMOS_URL" | head -n 1 | grep "200" > /dev/null && \
     curl -s --head "$GETH_URL" | head -n 1 | grep "200" > /dev/null; then

    echo "📥 Downloading Coinsspor snapshots..."
    echo "🔹 Cosmos: $LATEST_COSMOS"
    echo "🔸 Geth: $LATEST_GETH"

    # Download and extract snapshots
    curl "$COSMOS_URL" | lz4 -dc - | tar -xf - -C $HOME/.story/story
    curl "$GETH_URL" | lz4 -dc - | tar -xf - -C $HOME/.story/geth/aeneid/geth

    # Restore validator state (CRITICAL)
    mv $HOME/.story/priv_validator_state.json.backup $HOME/.story/story/data/priv_validator_state.json

    echo "✅ Coinsspor snapshot applied successfully!"
    
    # Restart services
    echo "🚀 Starting services..."
    sudo systemctl restart story-geth
    sleep 5
    sudo systemctl restart story

    echo "📊 Monitoring logs..."
    sudo journalctl -u story -u story-geth -f -o cat
  else
    echo "❌ Coinsspor snapshot URLs not accessible"
  fi
else
  echo "❌ No Coinsspor snapshots found"
fi
```

#### Service Information

| Feature | Details |
|---------|---------|
| **URL** | https://snaps.coinsspor.com/story/aeneid/ |
| **Update Frequency** | Every 6 hours |
| **Snapshot Type** | Pruned (optimal for validators) |
| **Components** | Story (Cosmos) + Geth (Execution) |
| **Compression** | LZ4 (fast download & extraction) |
| **Security** | Validator state preservation |
| **SSL** | ✅ HTTPS secured |

#### Benefits

- **🚀 Fast Sync**: 2-4 hours vs 2-7 days from genesis
- **💾 Space Efficient**: ~70% smaller than full snapshots  
- **🔒 Validator Safe**: Preserves priv_validator_state.json
- **⚡ High Speed**: Optimized compression and delivery
- **🔄 Always Fresh**: Updated every 6 hours
- **🌐 Reliable**: Professional infrastructure

#### Manual Download (Alternative)

If you prefer manual download, visit: https://snaps.coinsspor.com/story/aeneid/

Available files:
- `story_YYYYMMDD-HHMM_HEIGHT_cosmos.tar.lz4` - Story (Cosmos) data
- `story_YYYYMMDD-HHMM_HEIGHT_geth.tar.lz4` - Geth (Execution) data  
- `block-height.txt` - Current snapshot block height
## 🏁 Start Services

```bash
# Reload systemd and start services
sudo systemctl daemon-reload

# Enable services
sudo systemctl enable story-geth story

# Start story-geth first
sudo systemctl start story-geth

# Wait a few seconds then start story
sleep 5
sudo systemctl start story

# Check status
sudo systemctl status story-geth
sudo systemctl status story
```

---

## 🏁 Start Services

### Check Sync Status

```bash
# Check if node is catching up
curl localhost:${STORY_PORT}657/status | jq '.result.sync_info.catching_up'

# Get current block height
curl localhost:${STORY_PORT}657/status | jq '.result.sync_info.latest_block_height'

# Check geth sync status
story-geth --exec "eth.syncing" attach ~/.story/geth/aeneid/geth.ipc
```

### 📈 Live Sync Monitor Script

```bash
# Remove any existing broken monitor script
rm -f $HOME/monitor.sh

# Create new monitoring script
cat > $HOME/monitor.sh << 'EOF'
#!/bin/bash

# Get port from environment variable
source $HOME/.bash_profile
rpc_port="${STORY_PORT}657"
local_rpc="localhost:$rpc_port"
network_rpc="https://aeneid.storyrpc.io"

echo "🚀 Story Protocol Sync Monitor"
echo "=============================="
echo "Local RPC: $local_rpc"
echo "Network RPC: $network_rpc"
echo "Press Ctrl+C to stop monitoring"
echo "=============================="

while true; do
 local_height=$(curl -s "$local_rpc/status" | jq -r '.result.sync_info.latest_block_height')
 network_height=$(curl -s "$network_rpc/status" | jq -r '.result.sync_info.latest_block_height')

 if ! [[ "$local_height" =~ ^[0-9]+$ ]] || ! [[ "$network_height" =~ ^[0-9]+$ ]]; then
   echo -e "\033[1;31m❌ Error: Failed to fetch block heights. Retrying...\033[0m"
   sleep 5
   continue
 fi

 blocks_left=$((network_height - local_height))
 if [ "$blocks_left" -lt 0 ]; then
   blocks_left=0
 fi

 # Calculate sync percentage
 sync_percentage=$(echo "scale=2; ($local_height / $network_height) * 100" | bc -l 2>/dev/null || echo "0")

 echo -e "\033[1;33m📊 Node Height:\033[1;34m $local_height\033[0m \033[1;33m| 🌐 Network Height:\033[1;36m $network_height\033[0m \033[1;33m| ⏳ Blocks Left:\033[1;31m $blocks_left\033[0m \033[1;32m| 🔄 Sync: ${sync_percentage}%\033[0m"
 sleep 5
done
EOF

# Set execute permissions
chmod +x $HOME/monitor.sh

# Verify script was created successfully
echo "✅ Monitor script created successfully"
ls -la $HOME/monitor.sh

# Run the monitor
echo "🚀 Starting sync monitor..."
cd $HOME
./monitor.sh
```

---

## 🏆 Validator Creation

### Step 1: Export Validator Keys

```bash
# Export validator public key
story validator export

# Export EVM private key to file
story validator export --export-evm-key
```

### Step 2: Setup .env File

```bash
# Create .env file in the Story config directory (more secure and organized)
echo "PRIVATE_KEY=$(cat $HOME/.story/story/config/private_key.txt | grep "PRIVATE_KEY" | awk -F'=' '{print $2}')" > $HOME/.story/story/config/.env

# Verify .env file (DO NOT share this output)
echo "✅ .env file created in $HOME/.story/story/config/"
echo "⚠️  Keep your .env file secure and never share it!"

# Check if .env file is correctly formatted
if [ -f "$HOME/.story/story/config/.env" ]; then
    echo "✅ .env file exists in config directory"
else
    echo "❌ .env file creation failed"
    exit 1
fi

# Set proper permissions for security
chmod 600 $HOME/.story/story/config/.env
echo "✅ .env file permissions set to 600 (owner read/write only)"
```

### Step 3: Create Validator

Wait until your node is fully synced (`catching_up: false`), then create your validator:

```bash
# Check sync status first
curl localhost:${STORY_PORT}657/status | jq '.result.sync_info.catching_up'

# Navigate to the directory containing .env file
cd $HOME/.story/story/config/

# If catching_up is false, proceed with validator creation
# For LOCKED tokens (recommended for competition):
story validator create \
  --stake 1024000000000000000000 \
  --moniker "$STORY_MONIKER" \
  --chain-id 1315 \
  --unlocked=false \
  --rpc "https://aeneid.storyrpc.io"

# For UNLOCKED tokens:
story validator create \
  --stake 1024000000000000000000 \
  --moniker "$STORY_MONIKER" \
  --chain-id 1315 \
  --unlocked=true \
  --rpc "https://aeneid.storyrpc.io"
```

**Important Notes:**
- ✅ Story v1.2.0+ requires `.env` file for all operations
- ✅ The `.env` file must be in the same directory where you run commands
- ✅ Never commit or share your `.env` file
- ✅ Minimum stake is 1024 IP (1024000000000000000000 wei)

### Step 4: Delegate to Your Validator

```bash
# Get your validator public key
VALIDATOR_PUBKEY=$(story validator export | grep "Compressed Public Key (hex)" | awk '{print $NF}')

# Navigate to the directory containing .env file
cd $HOME/.story/story/config/

# Delegate additional stake
story validator stake \
  --chain-id 1315 \
  --validator-pubkey $VALIDATOR_PUBKEY \
  --stake 1000000000000000000000 \
  --rpc "https://aeneid.storyrpc.io"
```

---

## 🔧 Useful Commands

### 🔧 Service Management

```bash
# View logs
sudo journalctl -u story -f -o cat
sudo journalctl -u story-geth -f -o cat

# Restart services
sudo systemctl restart story
sudo systemctl restart story-geth

# Stop services
sudo systemctl stop story story-geth
```

### 📊 Node Information

```bash
# Get node ID
curl localhost:${STORY_PORT}657/status | jq '.result.node_info.id'

# Get validator address
story status 2>&1 | jq '.ValidatorInfo.address'

# Check validator status
curl localhost:${STORY_PORT}657/validators | jq '.result.validators[] | select(.address=="YOUR_VALIDATOR_ADDRESS")'

# Check node sync info
curl localhost:${STORY_PORT}657/status | jq '.result.sync_info'

# Check number of connected peers
curl -s localhost:${STORY_PORT}657/net_info | jq '.result.n_peers'

# Check peer details
curl -s localhost:${STORY_PORT}657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr | capture(":(?<port>[0-9]+)$").port) - \(.node_info.moniker)"'
```

### 🔄 Fresh Peers Management

```bash
# Create fresh peers function for easy use
cat > $HOME/get_fresh_peers.sh << 'EOF'
#!/bin/bash

get_fresh_peers() {
    echo "🔍 Fetching fresh peers from Coinsspor RPC..."
    URL="https://story-aeneid-testnet-rpc.coinsspor.com/net_info"
    response=$(curl -s $URL)
    
    if [ -z "$response" ]; then
        echo "❌ Failed to connect to RPC endpoint"
        return 1
    fi
    
    PEERS=$(echo $response | jq -r '.result.peers[] | select(.remote_ip | test("^[0-9]{1,3}(\\.[0-9]{1,3}){3}$")) | "\(.node_info.id)@\(.remote_ip):" + (.node_info.listen_addr | capture(":(?<port>[0-9]+)$").port)' | paste -sd "," -)
    
    if [ -n "$PEERS" ]; then
        echo "✅ Fresh peers found:"
        echo "$PEERS" | tr ',' '\n' | head -10  # Show first 10 peers
        echo ""
        echo "🔧 Apply these peers? (y/n)"
        read -r apply_peers
        
        if [ "$apply_peers" = "y" ] || [ "$apply_peers" = "Y" ]; then
            sed -i 's|^persistent_peers *=.*|persistent_peers = "'$PEERS'"|' $HOME/.story/story/config/config.toml
            sudo systemctl restart story
            echo "✅ Fresh peers applied and service restarted!"
            
            # Check connection after restart
            sleep 10
            peer_count=$(curl -s localhost:${STORY_PORT}657/net_info | jq '.result.n_peers')
            echo "📊 Connected to $peer_count peers"
        fi
    else
        echo "❌ No fresh peers found"
    fi
}

# Run the function
get_fresh_peers
EOF

chmod +x $HOME/get_fresh_peers.sh
echo "✅ Fresh peers script created: $HOME/get_fresh_peers.sh"
```

### 💰 Wallet Operations

```bash
# Navigate to the directory containing .env file
cd $HOME/.story/story/config/

# Check balance (replace YOUR_ADDRESS with your actual address)
curl -s "https://aeneid.storyrpc.io/cosmos/bank/v1beta1/balances/YOUR_ADDRESS" | jq

# Send tokens (replace addresses and adjust amount as needed)
story tx bank send YOUR_ADDRESS RECIPIENT_ADDRESS 1000000000000000000000uip \
  --chain-id 1315 \
  --rpc https://aeneid.storyrpc.io

# Check transaction status
curl -s "https://aeneid.storyrpc.io/cosmos/tx/v1beta1/txs/TX_HASH" | jq
```

### 🥩 Staking Operations

```bash
# Navigate to the directory containing .env file
cd $HOME/.story/story/config/

# Check delegations
story query staking delegations YOUR_DELEGATOR_ADDRESS --rpc https://aeneid.storyrpc.io

# Unstake tokens
story validator unstake \
  --validator-pubkey $VALIDATOR_PUBKEY \
  --unstake 1000000000000000000000 \
  --delegation-id 0 \
  --rpc "https://aeneid.storyrpc.io"

# Redelegate
story validator redelegate \
  --validator-src-pubkey $SOURCE_VALIDATOR_PUBKEY \
  --validator-dst-pubkey $DEST_VALIDATOR_PUBKEY \
  --redelegate 1000000000000000000000 \
  --rpc "https://aeneid.storyrpc.io"
```

---

## 🔄 Upgrades & Maintenance

### 📡 Where to Follow Upgrade Announcements

#### 1. **Official GitHub Releases** (Primary Source)
- **Story Consensus Client:** [https://github.com/piplabs/story/releases](https://github.com/piplabs/story/releases)
- **Story-Geth Execution Client:** [https://github.com/piplabs/story-geth/releases](https://github.com/piplabs/story-geth/releases)

#### 2. **Official Documentation** 
- **Release Notes:** [https://docs.story.foundation/network/releases](https://docs.story.foundation/network/releases)
- **Node Upgrade Guide:** [https://docs.story.foundation/network/operate/upgrade](https://docs.story.foundation/network/operate/upgrade)

#### 3. **Community Channels**
- **Discord:** [https://discord.gg/storyprotocol](https://discord.gg/storyprotocol) (announcements channel)
- **Telegram:** [https://t.me/storyprotocol](https://t.me/storyprotocol) (official announcements)
- **Twitter/X:** [@Story_Protocol](https://twitter.com/Story_Protocol) (follow for upgrade alerts)

### 🔔 Upgrade Types and Urgency

| Type | Description | Action Required | Timeline |
|------|-------------|-----------------|----------|
| **Major** | Hard fork upgrade with block height | **MANDATORY** before block height | Usually 1-2 weeks notice |
| **Minor** | Bug fixes, improvements | **RECOMMENDED** ASAP | Deploy within 24-48 hours |
| **Critical** | Security fixes | **URGENT** | Deploy immediately |
| **Optional** | Performance improvements | Optional | Deploy when convenient |

### ⚠️ **Important: Why Build from Source?**

**Pre-compiled binaries on GitHub are built with Ubuntu 24.04** and may not work properly on Ubuntu 22.04 due to library dependencies. **Building from source ensures compatibility** with your system.

#### ❌ **Pre-compiled Binaries Issue**
- Built for Ubuntu 24.04
- Different glibc versions
- Library dependency conflicts
- May cause crashes or failures

#### ✅ **Build from Source (Required for Ubuntu 22.04)**
- **Full compatibility** with your system
- **Correct library linking**
- **Stable operation**
- **No dependency issues**

### 🚀 Upgrade Methods

#### Method 1: Build for Cosmovisor Auto-Upgrade (Recommended)

```bash
# Get the latest release tag
LATEST_VERSION=$(curl -s https://api.github.com/repos/piplabs/story/releases/latest | jq -r '.tag_name')
echo "Latest version: $LATEST_VERSION"

# ⚠️ IMPORTANT: Set the upgrade height from official announcement
UPGRADE_HEIGHT="REPLACE_WITH_ACTUAL_HEIGHT"  # Example: 2065886

echo "⚠️ CRITICAL: Replace UPGRADE_HEIGHT with actual height from announcement!"
echo "📡 Check official channels for upgrade height"

# Validate upgrade height is set
if [ "$UPGRADE_HEIGHT" = "REPLACE_WITH_ACTUAL_HEIGHT" ]; then
    echo "❌ ERROR: You must set the actual upgrade height!"
    echo "📋 Current height: $(curl -s localhost:${STORY_PORT}657/status | jq -r '.result.sync_info.latest_block_height')"
    exit 1
fi

# Clone and build the new version
cd $HOME
rm -rf story-upgrade
git clone https://github.com/piplabs/story story-upgrade
cd story-upgrade
git checkout $LATEST_VERSION
go build -o story ./client

# Verify the build
./story version

# Create upgrade folder
mkdir -p $HOME/.story/story/cosmovisor/upgrades/$LATEST_VERSION/bin

# Copy built binary to upgrade directory
cp ./story $HOME/.story/story/cosmovisor/upgrades/$LATEST_VERSION/bin/

# Create upgrade info
echo "{\"name\":\"$LATEST_VERSION\",\"time\":\"0001-01-01T00:00:00Z\",\"height\":$UPGRADE_HEIGHT}" > $HOME/.story/story/cosmovisor/upgrades/$LATEST_VERSION/upgrade-info.json

# Setup automatic upgrade with Cosmovisor
cosmovisor add-upgrade $LATEST_VERSION $HOME/.story/story/cosmovisor/upgrades/$LATEST_VERSION/bin/story --force --upgrade-height $UPGRADE_HEIGHT

# Verify setup
echo "✅ Checking upgrade setup:"
ls -l $HOME/.story/story/cosmovisor/current
cat $HOME/.story/story/cosmovisor/upgrades/$LATEST_VERSION/upgrade-info.json

# Show status
CURRENT_HEIGHT=$(curl -s localhost:${STORY_PORT}657/status | jq -r '.result.sync_info.latest_block_height')
BLOCKS_REMAINING=$((UPGRADE_HEIGHT - CURRENT_HEIGHT))
echo "📊 Current Height: $CURRENT_HEIGHT"
echo "🎯 Upgrade Height: $UPGRADE_HEIGHT"  
echo "⏳ Blocks Remaining: $BLOCKS_REMAINING"

echo "✅ Automatic upgrade scheduled! Monitor with:"
echo "sudo journalctl -u story -f"

# Cleanup
rm -rf $HOME/story-upgrade
```

#### Method 2: Manual Build and Replace (Advanced Users)

```bash
# Stop the service
sudo systemctl stop story

# Get the latest release tag and build
LATEST_VERSION=$(curl -s https://api.github.com/repos/piplabs/story/releases/latest | jq -r '.tag_name')
echo "Building version: $LATEST_VERSION"

cd $HOME
rm -rf story-upgrade
git clone https://github.com/piplabs/story story-upgrade
cd story-upgrade
git checkout $LATEST_VERSION
go build -o story ./client

# Replace the main binary
sudo mv story $HOME/go/bin/story
sudo chmod +x $HOME/go/bin/story

# Update cosmovisor current binary
cosmovisor init $(which story)

# Verify version
story version

# Start the service
sudo systemctl start story

# Check if upgrade was successful
sudo journalctl -u story -n 50 --no-pager

# Cleanup
cd $HOME
rm -rf story-upgrade
```

---

## 🔍 Troubleshooting

### 🚨 Common Issues and Solutions

#### 🔧 Node Not Syncing

**Symptoms:**
- Node stuck at same block height
- `catching_up: true` for extended periods
- No new blocks being processed

**Solutions:**
```bash
# Check current sync status
curl localhost:${STORY_PORT}657/status | jq '.result.sync_info'

# Check peer connections
curl localhost:${STORY_PORT}657/net_info | jq '.result.n_peers'

# Get fresh peers and restart
./get_fresh_peers.sh

# Restart services
sudo systemctl restart story story-geth

# Check if geth is syncing
story-geth --exec "eth.syncing" attach ~/.story/geth/aeneid/geth.ipc
```

#### 💾 Disk Space Issues

**Symptoms:**
- Services crashing due to disk space
- `No space left on device` errors
- Node stops syncing

**Solutions:**
```bash
# Check disk usage
df -h
du -sh $HOME/.story/*

# Clean old logs
sudo journalctl --vacuum-time=3d

# Prune geth data (CAUTION: Will require re-sync)
sudo systemctl stop story-geth
story-geth --datadir ~/.story/geth/aeneid removedb
sudo systemctl start story-geth

# Check for large log files
find $HOME/.story -name "*.log" -size +100M
```

#### 🔌 Port Conflicts

**Symptoms:**
- Services fail to start
- `Address already in use` errors
- Cannot bind to port

**Solutions:**
```bash
# Check what's using your ports
sudo netstat -tulpn | grep :${STORY_PORT}657  # RPC
sudo netstat -tulpn | grep :${STORY_PORT}656  # P2P
sudo netstat -tulpn | grep :${STORY_PORT}545  # Geth RPC

# Kill conflicting processes
sudo kill -9 PID_NUMBER

# Change port prefix if needed
echo "export STORY_PORT=\"56\"" >> $HOME/.bash_profile
source $HOME/.bash_profile

# Reconfigure ports
sed -i.bak -e "s%:26657%:${STORY_PORT}657%g" $HOME/.story/story/config/config.toml
```

#### 🚫 Service Won't Start

**Symptoms:**
- `systemctl start` fails
- Service shows `failed` status
- Immediate crashes

**Solutions:**
```bash
# Check service status and logs
sudo systemctl status story
sudo systemctl status story-geth

# Check recent logs for errors
sudo journalctl -u story -n 50 --no-pager
sudo journalctl -u story-geth -n 50 --no-pager

# Check binary permissions
ls -la $HOME/go/bin/story*
chmod +x $HOME/go/bin/story
chmod +x $HOME/go/bin/story-geth

# Verify environment variables
echo "DAEMON_HOME: $DAEMON_HOME"
echo "STORY_PORT: $STORY_PORT"

# Test binary manually
$HOME/go/bin/story version
```

#### 🔄 Cosmovisor Issues

**Symptoms:**
- Upgrades not working
- Binary not found errors
- Version mismatches

**Solutions:**
```bash
# Check cosmovisor setup
cosmovisor version
ls -la $DAEMON_HOME/cosmovisor/

# Verify current binary
ls -la $DAEMON_HOME/cosmovisor/current/bin/

# Check upgrade directory
ls -la $DAEMON_HOME/cosmovisor/upgrades/

# Reset to genesis binary
cp $HOME/go/bin/story $DAEMON_HOME/cosmovisor/genesis/bin/

# Check environment
env | grep DAEMON
```

#### ⚠️ Validator Issues

**Symptoms:**
- Not signing blocks
- Validator offline
- Missing attestations

**Solutions:**
```bash
# Check validator status
curl localhost:${STORY_PORT}657/status | jq '.result.validator_info'

# Verify validator key
ls -la $HOME/.story/story/config/priv_validator_key.json

# Check if node is synced
curl localhost:${STORY_PORT}657/status | jq '.result.sync_info.catching_up'

# Monitor validator performance
curl localhost:${STORY_PORT}657/validators | jq '.result.validators[] | select(.address=="YOUR_VALIDATOR_ADDRESS")'

# Restart if needed
sudo systemctl restart story
```

### 📊 Log Analysis and Monitoring

#### Essential Log Commands
```bash
# Monitor real-time logs
sudo journalctl -u story -f
sudo journalctl -u story-geth -f

# Check for specific errors
sudo journalctl -u story --since "1 hour ago" | grep -i error
sudo journalctl -u story-geth --since "1 hour ago" | grep -i error

# Check connection issues
sudo journalctl -u story --since "1 hour ago" | grep -i "peer\|connection"

# Monitor resource usage
htop
iostat -x 1
```

#### Performance Monitoring Script
```bash
# Create monitoring script
cat > $HOME/performance_monitor.sh << 'EOF'
#!/bin/bash

echo "=== Story Node Performance Monitor ==="
echo "====================================="

# System resources
echo "📊 System Resources:"
echo "CPU Usage: $(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | awk -F'%' '{print $1}')%"
echo "Memory: $(free -h | awk '/^Mem:/ {print $3 "/" $2}')"
echo "Disk: $(df -h $HOME/.story | awk 'NR==2 {print $3 "/" $2 " (" $5 " used)"}')"

# Node status
echo ""
echo "🌐 Node Status:"
SYNC_INFO=$(curl -s localhost:${STORY_PORT}657/status)
HEIGHT=$(echo $SYNC_INFO | jq -r '.result.sync_info.latest_block_height')
CATCHING_UP=$(echo $SYNC_INFO | jq -r '.result.sync_info.catching_up')
PEERS=$(curl -s localhost:${STORY_PORT}657/net_info | jq -r '.result.n_peers')

echo "Block Height: $HEIGHT"
echo "Catching Up: $CATCHING_UP"
echo "Connected Peers: $PEERS"

# Service status
echo ""
echo "🔧 Service Status:"
STORY_STATUS=$(systemctl is-active story)
GETH_STATUS=$(systemctl is-active story-geth)
echo "Story Service: $STORY_STATUS"
echo "Geth Service: $GETH_STATUS"

echo "====================================="
EOF

chmod +x $HOME/performance_monitor.sh
```

### 🔧 Advanced Troubleshooting

#### Complete Node Reset
```bash
# ⚠️ WARNING: This will delete all data and require full re-sync
echo "⚠️ WARNING: This will delete ALL node data!"
echo "Make sure you have backed up your validator keys!"
read -p "Continue? (y/N): " confirm

if [ "$confirm" = "y" ]; then
    # Stop services
    sudo systemctl stop story story-geth
    
    # Backup validator keys
    cp $HOME/.story/story/config/priv_validator_key.json $HOME/validator_key_backup.json
    cp $HOME/.story/story/config/node_key.json $HOME/node_key_backup.json
    
    # Remove data directories
    rm -rf $HOME/.story/story/data
    rm -rf $HOME/.story/geth/aeneid/geth/chaindata
    
    # Reinitialize
    story init --moniker $STORY_MONIKER --network aeneid
    
    # Restore keys
    cp $HOME/validator_key_backup.json $HOME/.story/story/config/priv_validator_key.json
    cp $HOME/node_key_backup.json $HOME/.story/story/config/node_key.json
    
    # Restart services
    sudo systemctl start story-geth
    sleep 5
    sudo systemctl start story
    
    echo "✅ Node reset complete. Monitor sync with: ./monitor.sh"
fi
```

#### Network Connectivity Test
```bash
# Test network connectivity
cat > $HOME/network_test.sh << 'EOF'
#!/bin/bash

echo "🌐 Network Connectivity Test"
echo "=========================="

# Test RPC endpoints
echo "Testing RPC endpoints..."
curl -s --max-time 5 https://aeneid.storyrpc.io/status >/dev/null && echo "✅ Official RPC: OK" || echo "❌ Official RPC: Failed"
curl -s --max-time 5 https://story-aeneid-testnet-rpc.coinsspor.com/status >/dev/null && echo "✅ Coinsspor RPC: OK" || echo "❌ Coinsspor RPC: Failed"

# Test local services
echo ""
echo "Testing local services..."
curl -s --max-time 5 localhost:${STORY_PORT}657/status >/dev/null && echo "✅ Local Story RPC: OK" || echo "❌ Local Story RPC: Failed"
curl -s --max-time 5 localhost:${STORY_PORT}545 -X POST -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' >/dev/null && echo "✅ Local Geth RPC: OK" || echo "❌ Local Geth RPC: Failed"

# Test DNS resolution
echo ""
echo "Testing DNS resolution..."
nslookup aeneid.storyrpc.io >/dev/null && echo "✅ DNS Resolution: OK" || echo "❌ DNS Resolution: Failed"

echo "=========================="
EOF

chmod +x $HOME/network_test.sh
./network_test.sh
```

### 🆘 Emergency Procedures

#### Emergency Validator Recovery
```bash
# If validator is offline and you need quick recovery
echo "🆘 Emergency Validator Recovery"

# 1. Check if keys exist
if [ ! -f "$HOME/.story/story/config/priv_validator_key.json" ]; then
    echo "❌ Validator key not found! Restore from backup immediately!"
    exit 1
fi

# 2. Force restart everything
sudo systemctl stop story story-geth
sleep 5
sudo systemctl start story-geth
sleep 10
sudo systemctl start story

# 3. Monitor recovery
echo "📊 Monitoring recovery..."
for i in {1..30}; do
    CATCHING_UP=$(curl -s localhost:${STORY_PORT}657/status | jq -r '.result.sync_info.catching_up')
    echo "Attempt $i/30 - Catching up: $CATCHING_UP"
    if [ "$CATCHING_UP" = "false" ]; then
        echo "✅ Node synced! Checking validator status..."
        break
    fi
    sleep 10
done
```

#### Emergency Contact Information
```bash
echo "🆘 Emergency Support Contacts:"
echo "=============================="
echo "Discord: https://discord.gg/storyprotocol"
echo "Telegram: https://t.me/storyprotocol"
echo "GitHub Issues: https://github.com/piplabs/story/issues"
echo "Coinsspor Support: https://t.me/coinsspor"
echo "=============================="
```

---

## 🧹 Complete Node Removal

```bash
# Stop services
sudo systemctl stop story story-geth
sudo systemctl disable story story-geth

# Remove service files
sudo rm /etc/systemd/system/story.service
sudo rm /etc/systemd/system/story-geth.service
sudo systemctl daemon-reload

# Remove data directories
rm -rf $HOME/.story
rm -rf $HOME/story
rm -rf $HOME/story-geth

# Remove binaries
rm $HOME/go/bin/story
rm $HOME/go/bin/story-geth
```

---

## 📞 Support & Community

| Platform | Link | Description |
|----------|------|-------------|
| **Discord** | [https://discord.gg/storyprotocol](https://discord.gg/storyprotocol) | Community Support |
| **Telegram** | [https://t.me/storyprotocol](https://t.me/storyprotocol) | Official Announcements |
| **Documentation** | [https://docs.story.foundation](https://docs.story.foundation) | Technical Documentation |
| **GitHub** | [https://github.com/piplabs](https://github.com/piplabs) | Source Code & Issues |

---

## 🎯 Quick Start Summary

1. **🔧 Install dependencies and Go 1.22.11**
2. **⚙️ Set moniker and port prefix variables**
3. **🏗️ Build story-geth v1.1.1 and story v1.3.0**
4. **🔄 Setup Cosmovisor for automatic upgrades**
5. **🌐 Initialize node for Aeneid testnet**
6. **📡 Configure dynamic ports, peers, and monitoring**
7. **🚀 Create and start systemd services**
8. **⏳ Wait for sync completion**
9. **🏆 Create validator with 1024+ IP stake**
10. **📊 Monitor performance and participate in competition**

---

**🚀 Good luck with your Story Protocol validator! 🚀**

---

*This guide is maintained by the [Coinsspor](https://coinsspor.com) team. For updates and support, visit our endpoints and community channels.*
