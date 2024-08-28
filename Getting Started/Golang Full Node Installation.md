# Karlsen: Full Node Installation Guide

Karlsen is a reference full node implementation of the Karlsen network, originally developed in Go (Golang).

**For the KarlsenHashv2 mainnet, the recommended software is the Rust node** due to its superior performance, scalability, and security features compared to the [Golang node](https://github.com/karlsen-network/karlsend). You can download the Rust node [here](https://github.com/karlsen-network/rusty-karlsen/releases) and follow the installation guide [Rust Full Node Installation](Getting%20Started/README.md).

## Compilation Requirements

If you want to compile the binaries yourself (if you aren't sure whether you want to, then you probably don't), you would need Go 1.16 or later.

## Setup

Mining Karlsen requires two components: a node (karlsend), and a miner. A third component is required to create and maintain a wallet. The node listens for new blocks while the miner is searching for blocks to report to the node. All three components are provided as stand alone files which require no installation. 

You need to either download precompiled binaries, or compile the codebase yourself. The first option is recommended for most users.

Note that all karlsend and the miner must be running in parallel. That is, each should be running from a different console and should not be distured as long as mining takes place.

#### Download Binaries

The easiest way to use karlsend is to download the binaries from [here](https://github.com/karlsennet/karlsend/releases/latest). After downloading the binaries that fit your operating system, you should extract them to some folder.

Notice that the rest of the tutorial assumes that you installed from source, so before each command you run you should first run: 
```bash
$ cd <THE_EXTRACTED_BINARIES_FOLDER>
```

Linux and Mac users might need to add `./` to any command so it'll run the corresponding binary. For example:
```bash
./karlsend --utxoindex
```


#### Build from Source

- Install Go according to the installation instructions here:
  http://golang.org/doc/install

- Ensure Go was installed properly and is a supported version:

```bash
$ go version
```

- Run the following commands to obtain and install karlsend including all dependencies:

```bash
$ git clone https://github.com/karlsen-network/karlsend
$ cd karlsend
$ go install . ./cmd/...
```

- Karlsend (and utilities) should now be installed in `$(go env GOPATH)/bin`. If you did
  not already add the bin directory to your system path during Go installation,
  you are encouraged to do so now.

## Getting Started

Karlsend has several configuration options available to tweak how it runs, but all
of the basic operations work with zero configuration except the `--utxoindex` flag (you can omit this flag if you don't use the wallet):

```bash
$ karlsend --utxoindex
```

You can invoke ```karlsend --help``` to get a list of more running flags.

The first time you run karlsend it will retrieve peer information from Karlsen's DNS server and will start synchronizing with the network. First synchronization may take up to several hours (depending on your CPU strength and bandwidth). It is impossible to mine before the network is synced. Every time you run karlsend it will incrementally sync any blocks accumulated while it was offline, this is typically a much shorter process (as long as karlsend was not shut down for more than several hours).

### Creating a Wallet (optional)

To run a miner you need to create a keypair to mine into:
```bash
$ karlsenwallet create
```

You will be asked to choose a password for the wallet (a password must be at least 8 characters long, and it won't be shown on the screen you as you entering it). After that you should run this command in order to start the wallet daemon:
```bash
$ karlsenwallet start-daemon
```

And then run this in order to request an address from the wallet:
```bash
$ karlsenwallet new-address
```

Your screen will show you something like this:
```
The wallet address is:
karlsen:0123456789abcdef0123456789abcdef0123456789
```

**Note**: Every time you ask karlsenwallet for an address you will get a different address. This is perfectly fine. Every secret key is associated with many different public addresses and there is no reason not to use a fresh one for each transaction.

At this point your can close the wallet daemon, though you should keep it running of you want to be able to check your balance and make transactions

### Running a Miner (Optional)

**Note:** The Golang-based built-in CPU miner has been significantly outperformed by newer GPU miners. For optimal performance, we recommend using our official [Karlsen Rust-Based GPU Miner](https://github.com/karlsen-network/karlsen-miner) or one of the trusted third-party GPU miners listed below.

Available miners include:
- [lolMiner](https://github.com/Lolliedieb/lolMiner-releases)
- [Team Red Miner](https://github.com/todxx/teamredminer)
- [SRBMiner](https://github.com/doktor83/SRBMiner-Multi)
- [BzMiner](https://github.com/bzminer/bzminer)
- [Rigel](https://github.com/rigelminer/rigel)
- [GMiner](https://github.com/develsoftware/GMinerRelease)

To start mining, copy the address generated by your wallet and run the miner with it:
```bash
$ karlsenminer --miningaddr karlsen:<YOUR_CREATED_ADDRESS>
```

**Note:** Mining will only start after the network is fully synchronized. The miner will wait until the node is synced, which may result in seeing 0 Hashes/second initially.

### Mining on Additional Computers

You can mine using additional computers by pointing them to your main node:

```bash
$ karlsenminer -s <node IP address> --miningaddr karlsen:<YOUR_CREATED_ADDRESS>
```

Find your node's IP address using `ifconfig` on Linux/Mac or `ipconfig` on Windows.

### Karlsen Rust-Based GPU Miner with Karlsenhashv2 Support

For enhanced performance, use the [Karlsen Rust-based GPU miner](https://github.com/karlsen-network/karlsen-miner).

To run the miner:
```bash
$ karlsen-miner-v2.0.0-win64-amd64 --karlsend-address <node IP address> --mining-address <wallet address>
```

If running the miner on the same machine as karlsend, you can omit the `--karlsend-address` flag, as it defaults to localhost.

**Note for Windows users:** The GPU miner binary is unsigned, which might cause it to be blocked by Windows Defender. You may need to manually add an exclusion.

**Note for Linux/Mac users:** If you encounter a `permission denied` error, fix it by making the file executable:
```bash
chmod +x <file name>
```

### Opening Ports

By forwarding port 42111 (unless configured otherwise) to the machine running karlsend, your node becomes a public node which other members of the network can use to sync. Even though private nodes can still mine, it is encouraged that you make your node public for the general health of the network. Like any other decentralized systems, Karlsen works best when there are many public nodes.

### Karlsend Hardware Requirements

**Minimum:**
- 100 GB disk space
- 7th generation i7 4-core processor or AMD equivalent
- 32GB memory
- 10 Mbit internet connection

**Recommended:**
- 9th generation i7 8-core processor or AMD equivalent
- 64 GB memory
- 40 Mbit internet connection

### Discord

[![Join the Karlsen Discord Server](https://img.shields.io/discord/1169939685280337930.svg?label=&logo=discord&logoColor=ffffff)](https://discord.gg/ZPZRvgMJDT)

You may want to join our discord server for further questions: https://discord.gg/ZPZRvgMJDT
