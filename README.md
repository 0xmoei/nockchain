## Nockchain
Nockchain is a lightweight chain for heavyweight compute. It uses ZK-Proof of Work (zkPoW). Miners create a ZK-Proof (ZKP) of a fixed puzzle computation, then hash the ZKP, and earn $NOCK based on their computation power.

The team has released a Public Testnet to run a ***local testnet node*** and a ***testnet miner***, to explore how Nockchain works before Mainnet goes live.

---

## $NOCK Details
- Genesis Mainnet and NOCK Mining starts May 21th.
- 10-min block times (like Bitcoin).
- Total Supply: 2^32 nocks (around 4.29 billion).
- Fair launch: 100% of $NOCK will be issued to Miners.
- $NOCK is used to pay for blockspace on Nockchain.

---

## Nock Mining Dashboard
This community-driven dashboard, [**NockStats**](https://nockstats.com/) has many good features like: Wallet balance, Trading, Tokenomics, Calculator, etc.

---

## How to Mine
* The mining terms is exactly same a Bitcoin PoW mining.
* More computational power = More rewards
* More miners on the network = Higher hashrate = More difficulty to mine rewards

### Method 1: Solo (CLI):
* You run a solo CPU-based Miner Node on your system which will certainly need a powerful hardware.
* You can follow [CLI Setup](https://github.com/0xmoei/nockchain/blob/main/README.md#cli-miner-setup) steps.

### Method 2: Pool & Method 3: GUI
* You join a pool (same as bitcoin mining), share your computational power and earn rewards in a shared pool of rewards.
* Official mining method is CLI introduced by the team, and no official Pool or GUI mining programs introduced.
* But new community-driven *Projects* and *Pools* are poping up.
* **GUI Setup (App)**: There's a new project called [**Nockpool**](https://swps.io/nockpool), I think they are building a GUI-based Node so you can easily open a wallet on Windows, join a pool, and Mine $NOCK.

![image](https://github.com/user-attachments/assets/6f58647d-2255-4ebb-839c-eeb539cac258)

---

# CLI Miner Setup
## Hardware Requirements
Hardware requirement is highly speculative since no one knows how Mainnet-launch goes.

<table>
  <tr>
    <th colspan="3">Miner is CPU-based initially</th>
  </tr>
  <tr>
    <td>RAM</td>
    <td>CPU</td>
    <td>Disk</td>
  </tr>
  <tr>
    <td><code>64 GB</code></td>
    <td><code>6 cores or higher</code></td>
    <td><code>100-200 GB SSD</code></td>
  </tr>
</table>

* more CPU = more hashrate = more chances
* We still can't say that's enough, because it's just a testnet environment and we need to wait until mainnet.

---

## OS Requirements

* **Windows Users:** Install Linux Ubuntu on your Windows using this [Guide](https://github.com/0xmoei/Install-Linux-on-Windows)
* **VPS:** Recommended crypto-payment VPS provider to [Purchase](https://my.hostbrr.com/order/forms/a/NTMxNw==) or Visit [VPS Beginner Guide](https://github.com/0xmoei/Linux_Node_Guide/)

> Note: Miners are initially CPU-based for users and will eventually move to GPU and ASIC later

---

## CLI Installation Steps
### Step 1: Install Dependecies
* Update Packages
```bash
sudo apt-get update && sudo apt-get upgrade -y
```
* Install Packages:
```bash
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev libclang-dev llvm-dev -y
```
* Rust:
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
```bash
source $HOME/.cargo/env
```
* Enable memory overcommit:
```bash
sudo sysctl -w vm.overcommit_memory=1
```

### Step 2: Delete Old Repo and Wipe All Data
```bash
# kill miner screen
screen -XS miner quit

# remove nockchain
rm -rf nockchain
rm -rf .nockapp
```

### Step 3: NockChain Repo
```bash
git clone https://github.com/zorp-corp/nockchain

cd nockchain
```
  
### Step 4: Build
* Copy `.env` file from the example one:
```bash
cp .env_example .env
```

* Build: (takes time depending on your hardware.)
```bash
make install-hoonc

export PATH="$HOME/.cargo/bin:$PATH"
```
```bash
make build
```
```bash
make install-nockchain-wallet

export PATH="$HOME/.cargo/bin:$PATH"
```
```bash
make install-nockchain

export PATH="$HOME/.cargo/bin:$PATH"
```

## Step 5: Setup Wallet
Make sure you are in `nockchain` directory.
* **Set PATH:**
```bash
export PATH="$HOME/.cargo/bin:$PATH"
```
* **Create wallet:**
```bash
nockchain-wallet keygen
```
* Save `memo`, `private key` & `public key` of your wallet.
> Note: After every terminal restart, Ensure you execute these two commands before executing wallet commands again: `cd nockchain` & `export PATH="$HOME/.cargo/bin:$PATH"`.  By doing this, you won't get Error: `wallet: command not found`.

* Replace the value of `MINING_PUBKEY=` in `.env` with your own Public Key:
```bash
nano .env
```
To save: Press `Ctrl + X`, `Y` & `Enter`.

## Step 6: Backup/Import Wallet Keys
Backup wallet keys:
```bash
nockchain-wallet export-keys
```
* This will save your keys to a file called `keys.export` in the current directory.

Import wallet keys:
```bash
nockchain-wallet import-keys --input keys.export
```
* Make sure `keys.export` is in your `nockchain` directory.

* Note: For Local systems who are using a home router network which is mostly behind NAT, they need to forward ports. Ask chatgpt until I get the chance to write a guide for it.

### Step 7: Run Miner
* Enable memory overcommit with this: `sudo sysctl -w vm.overcommit_memory=1`
* First, Make sure you are in nockchain directory: `cd ~/nockchain`

* The following method will be for running multiple miner instances on a server, you can repeat it by inscreasing `n` numbers by `n+1`

Run Miner 1:
```console
# create directory
mkdir miner1 && cd miner1

# open screen
screen -S miner1

# start miner
RUST_LOG=info,nockchain=info,nockchain_libp2p_io=info,libp2p=info,libp2p_quic=info \
MINIMAL_LOG_FORMAT=true \
nockchain --mining-pubkey PUBLIC_KEY --mine
```
* Replace `PUBLIC_KEY`.
* If you are getting `generating new candidate`, then you are sucessfully mining blocks!
* No worry if you are getting Errors like `command: timer`, `ConnectionError`, etc.
* To minimize screen:  `Ctrl` + `A` + `D`


### Useful Commands:
Restart Miner:
* If you ever stopped your Miner, to restart it again, delete old data files first:
```bash
rm -rf ./.data.nockchain .socket/nockchain_npc.sock
```
* Make sure you are in the specific miner's directory you want to restart .e.g `miner1`, `miner2`, etc.

Screen commands
* Ensure screens do not overlap. Before opening or switching to another screen, minimize or close the current screen.
* Replace `miner` with your miner's screen name .e.g `miner1`, `miner2`, etc.
```console
# Return screen
screen -r miner

# Minimize screen
Press: CTRL + A + D

# Screens list
screen -ls

# Stop Node (when inside a screen)
Press: Ctrl + C

# Kill and Remove Node (when outside a screen)
screen -XS miner quit
```

---

## FAQ
* Can I use same pubkey if running multiple miners?
  * Yes, you can use the same pubkey if running multiple miners.

* How do I change the mining pubkey?
  * Run nockchain-wallet keygen to generate a new key pair and copy the new public key to the .env file.
 
---

# Mining Genesis Block [Deprecated]
############### Deprecated -- Genesis Block is Mined already ##################
## Step 1: Get BTC Mainnet RPC -- (Mine Genesis Block)
If you want to join from **Genesis Block #0**, then you need to connect your Mined to a BTC corenode.
* 1- Get your free and private BTC Mainnet RPC from [QuickNode](https://dashboard.quicknode.com/)
  * Save your `RPC_URL` + `RPC_TOKEN`

![image](https://github.com/user-attachments/assets/931921cf-7bd0-43d4-81f6-d21514b9c807)


* 2- Check your RPC sync and connection status:
```
curl -X POST https://long-quick-shape.btc.quiknode.pro/{your_rpc_token} \
-d '{
      "id": "hmm",
      "method": "getindexinfo",
      "params": []
}'
```
* Replace `https://long-quick-shape.btc.quiknode.pro/{your_rpc_token}` with your bitcoin `RPC_URL` + `RPC_TOKEN`.

## Step 2: nginx Proxy for Bitcoin JSON-RPC
```
sudo apt update
sudo apt install nginx
```
```
sudo nano -p /etc/nginx/conf.d/btc-proxy.conf
```
* Paste this. Replace `RPC_URL` & `RPC_TOKEN`
```server {
    listen 8332;

    location / {
        proxy_pass https://RPC_URL/RPC_TOKEN/;
        proxy_set_header Host https://RPC_URL;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        proxy_ssl_protocols TLSv1.2 TLSv1.3;
        proxy_ssl_server_name on;
    }
}
```
```
sudo nginx -t
```
```
sudo systemctl reload nginx
```
```
sudo nginx -s reload
```

## Step 3: Get local BTC RPC
* Your Bitcoin RPC client is now: `http://127.0.0.1:8332/`

* Try a simple request to get block number:
```bash
curl -v -X POST http://127.0.0.1:8332/ \
  -H 'content-type: application/json' \
  -d '{
      "id": "test",
      "method": "getblockchaininfo",
      "params": []
  }'
```

## Step 4: Run Miner (Genesis Mine)
```
screen -S miner
```
* Replace: `BTC_TOKEN` & `PUB_KEY`:
```
nockchain --btc-node-url="http://127.0.0.1:8332/" --btc-username="BTC_TOKEN" --btc-password="x" --genesis-watcher --mine --mining-pubkey="PUB_KEY"
```

Now continue from Step: [Useful commands](https://github.com/0xmoei/nockchain/blob/main/README.md#useful-commands)
