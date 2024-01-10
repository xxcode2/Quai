# Quai

 *Global Node*
 -Fast CPU with 8+ cores
 -32GB+ RAM
 -Fast SSD with at least 3TB free space
 -10+ MBit/sec download Internet service

 *Slice Node*
 -CPU with 4+ cores
 -16GB RAM
 -1TB free storage space to sync
 -10+ MBit/sec download Internet service

 
 ## Install Go v1.21.0+
 ```
# install snapd if you don't have it already
sudo apt install snapd

# install go
sudo snap install go --classic
```

## Git & Make
```
sudo apt install git make
```

## Install go-quai
```
git clone https://github.com/dominant-strategies/go-quai
cd go-quai
```
You can find the latest release on the go-quai releases page. Then, check out the latest release with:
```
git checkout put-latest-release-here
```
```
git checkout v1.2.3-rc.4
```
## Node Configuration
```
cp network.env.dist network.env
```
```
# edit network.env with nano
nano network.env

# open go-quai in vscode to edit network.env
code .

# edit network.env with vim
vim network.env
```
If you are mining, replace the default coinbase addresses below with your own for the chains that you intend to mine. You can generate addresses for each shard easily with Pelagus Wallet or Koala Wallet.

You must generate a unique address for each COINBASE that is maps to a shard, i.e. generate a Cyprus-1 address with Pelagus and set it as the ZONE_0_0_COINBASE below. Follow the same process for each chain you want to mine.

```
# Replace addresses below with your addresses for each shard
ZONE_0_0_COINBASE=0x04a3e45aa16163F2663015b6695894D918866d19 # cyprus1
ZONE_0_1_COINBASE=0x21c7650E65b164B2ab645eAF1141b569B2c82Bd7 # cyprus2
ZONE_0_2_COINBASE=0x3e742F0AE63d62304153526A51EE8BF0531d1887 # cyprus3
ZONE_1_0_COINBASE=0x72c871f639ed156De0b97C1b533e2617730b7ec2 # paxos1
ZONE_1_1_COINBASE=0x755c3603c5688CF3105F1a1AfC11fa0e1981b03B # paxos2
ZONE_1_2_COINBASE=0x9fF7B1A33BB22b70F5fb78A420CEd9075C437c87 # paxos3
ZONE_2_0_COINBASE=0xb8094B2bc411942fd078D994Cc4e9418D6A5B071 # hydra1
ZONE_2_1_COINBASE=0xC83cb918dd9267a344B51f57e28e8cf977057E05 # hydra2
ZONE_2_2_COINBASE=0xF39E7d05B5A1a2F934cC43221383f29e4794c822 # hydra3
```
```
ENABLE_ARCHIVE=true
```

## Router Configuration
```
# Networking Variables
ENABLE_NAT=true
EXT_IP=SERVER IP or WAN IP
```

## Starting a Node
# Build
```
make go-quai
```
# Start
```
make run
```


# Run a Slice Node
```
SLICES="[FIRST_REGION_INDEX FIRST_ZONE_INDEX],[SECOND_REGION_INDEX SECOND_ZONE_INDEX]"
```
After configuring the slices you'd like to run in your network.env, start your node using the same command by running:

```
make run
```

# Logs
```
tail -f nodelogs/prime.log
# OR
tail -f nodelogs/region-2.log
# OR
tail -f nodelogs/zone-0-0.log
```
The outputs of a node that has started correctly should look similar to below.

```
INFO [01-30|19:55:15.397] Starting Quai 0.1.0-pre.0 on local testnet
INFO [01-30|19:55:15.414] Maximum peer count                       ETH=50 total=50
WARN [01-30|19:55:15.460] Disable transaction unindexing for archive node
INFO [01-30|19:55:15.460] Enabling recording of key preimages since archive mode is used
INFO [01-30|19:55:15.460] Set global gas cap                       cap=50,000,000
INFO [01-30|19:55:15.460] Allocated trie memory caches             clean=307.00MiB dirty=0.00B
INFO [01-30|19:55:15.461] Allocated cache and file handles         database=/Users/user/Library/Quai/local/zone-0-0/quai/chaindata cache=512.00MiB handles=5120
INFO [01-30|19:55:17.727] Opened ancient database                  database=/Users/user/Library/Quai/local/zone-0-0/quai/chaindata/ancient readonly=false
INFO [01-30|19:55:17.728] Writing custom genesis block
```

To stop log outputs to the terminal, you can use CTRL+C

# Syncing
```
# Print all appended blocks
cat nodelogs/location-to-print-here.log | grep Appended

# Continuously print new appended blocks
tail -f nodelogs/location-to-print-here.log | grep Appended

# Continuously print appended blocks across all chains
tail -f nodelogs/* | grep Appended
```
The output should look similar to below:
```
INFO   [09-18|10:18:17.273] Appended new block                       number=[102 1934 40392] hash=0x0000067368b679ce7994dbd6e3dfe93a5e5fe16642a6083604fd405556836cbe difficulty=405369 uncles=0 txs=0 etxs=0 gas=0 gasLimit=5000000 root=0x7df4c77d1463a5e4c7d5f5446476e34df01cf14b6226b7d83ccab072bc302edc order=2 location=[0 0] elapsed=2.019ms
INFO   [09-18|10:18:17.736] Appended new block                       number=[102 1934 40393] hash=0x0000285b7ffa020c8f9f5f8832381593170d1d7618ad2fae8202350a0d81acac difficulty=405875 uncles=0 txs=0 etxs=0 gas=0 gasLimit=5000000 root=0x81954cf5d93a979890641acffe7496965ff4602ad2b24d24ab5356ba52072c39 order=2 location=[0 0] elapsed=1.933ms
INFO   [09-18|10:18:17.803] Appended new block                       number=[102 1934 40394] hash=0x00000d6f0d100a8d254088090876a6ab911720af7e7bc6454f5d1a01417f786f difficulty=406382 uncles=0 txs=0 etxs=0 gas=0 gasLimit=5000000 root=0x8eb0b430e2df8f91a180b6f29fea46430c9014ccde42fa538df62bf3251dff03 order=2 location=[0 0] elapsed=2.005ms
INFO   [09-18|10:18:18.511] Appended new block                       number=[102 1934 40395] hash=0x00001211f391c0a162701ad0dcbdef47f4efe96b3fb5f77e1f0b75b6ff439312 difficulty=406889 uncles=0 txs=0 etxs=0 gas=0 gasLimit=5000000 root=0xc810b3d05f9a9b7f4fee3da271d3544cba26b6368f84ee5e5e885cbe4fd11cab order=2 location=[0 0] elapsed=2.147ms
```

# Stop
```
make stop
```

# Updating
```
git fetch --all
```
Checkout the latest release of go-quai:
```
git checkout put-latest-release-here
```
Finally, rebuild the source using:
```
make go-quai
```

# Archiving And Syncing From a Snapshot
```
wget http://backup.colosseum.quai.network/quai_colosseum_backup.tar.zst
```
#Linux Machines

You can backup your node's database by running:
```
# Stop node
make stop

# Remove peer database
rm -rf ~/.quai/*/quai/nodes

# Remove nodekey
rm -rf ~/.quai/*/quai/nodekey

# Remove database lock
rm -rf ~/.quai/*/quai/LOCK

# install zstd if you don't have it already
sudo apt install zstd

# Archive and compress database
tar -I 'zstd -T0' -cvf quai_colosseum_backup.tar.zst .quai
```
To restore your database from a snapshot on Linux, use:

```
# Stop node
make stop

# Remove current db
# This command will permanently delete all state that you have synced thus far
rm -rf ~/.quai

# install zstd if you don't have it already
sudo apt install zstd

# Expand compressed db into node
tar -I 'zstd -T0' -xvf quai_colosseum_backup.tar.zst

# Copy db into db directory
cp -r quai-colosseum-backup ~/.quai
```
# Restart/Clearing
```
rm -rf nodelogs ~/.quai
```
