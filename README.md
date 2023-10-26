# Docknode BNB

1. Prerequisites

- `apt update && apt upgrade -y && apt install -y git unzip lz4`
- `git clone https://github.com/olivbau/node-bnb && cd node-bnb`
- Install caddy: https://caddyserver.com/docs/install

2. Configure environement variables

```bash
cp .env.example .env

# Generate passwords for basic auth
caddy hash-password --plaintext 'my_password'

# Set the host
nano .env
```

3. Download BNB Geth

```bash
wget $(curl -s https://api.github.com/repos/bnb-chain/bsc/releases/latest |grep browser_ |grep geth_linux |cut -d\" -f4)
mv geth_linux geth
chmod -v u+x geth
```

4. Setup Snapshot (up to 24 hours)

```bash
wget -O geth.tar.lz4 https://pub-c0627345c16f47ab858c9469133073a8.r2.dev/geth-20231012.tar.lz4
tar -I lz4 -xvf geth.tar.lz4
```

5. Run

```bash
./geth --config ./bnb/config.toml --datadir ./server/data-seed  --cache 8000 --rpc.allow-unprotected-txs --txlookuplimit 0
```