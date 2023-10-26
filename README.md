# Node BNB

1. Prerequisites

- Install caddy: https://caddyserver.com/docs/install
- `sudo apt update && sudo apt upgrade -y && sudo apt install -y git unzip lz4`
- `git clone https://github.com/olivbau/node-bnb && cd node-bnb`

2. Run Caddy

```bash
# Set environment variables
cp .env.example .env

# Generate passwords for basic auth
caddy hash-password --plaintext 'my_password'

# Set users and passwords for basic auth
# Set host
nano .env

# Start caddy server
caddy start --config Caddyfile --envfile .env

# Run caddy server
caddy stop
```

3. Download BNB Geth

```bash
wget $(curl -s https://api.github.com/repos/bnb-chain/bsc/releases/latest |grep browser_ |grep geth_linux |cut -d\" -f4)
mv geth_linux geth
chmod -v u+x geth
```

4. Setup Snapshot (up to 24 hours)

```bash
# Using tools such screen is recommanded
# https://github.com/bnb-chain/bsc-snapshots
wget -O geth.tar.lz4 "<paste snapshot URL here>"
tar -I lz4 -xvf geth.tar.lz4
```

5. Setup UFW

```bash
ufw allow ssh
sudo ufw allow http
sudo ufw allow https
ufw allow 30311
ufw deny 8545
ufw enable
```

6. Run

```bash
# Start a screen session
screen -S geth-bnb

./geth --config ./bnb/config.toml --datadir ./server/data-seed  --cache 8000 --rpc.allow-unprotected-txs --txlookuplimit 0 --http --http.port 8545 --http.vhosts=* --http.addr "0.0.0.0"

# Detach from a screen session
# ctrl + a + d
```

6. Useful commands

```bash
# List screen sessions
screen -ls

# Attach to a screen session
screen -r geth-bnb

# Kill a screen session
screen -X -S geth-bnb quit

# Detach from a screen session
# ctrl + a + d
```
