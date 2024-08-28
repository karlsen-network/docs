# Rusty-Karlsen: Full Node Installation Guide
[![GitHub release](https://img.shields.io/github/v/release/karlsen-network/rusty-karlsen.svg)](https://github.com/karlsen-network/rusty-karlsen/releases)

**The Rust node is the recommended software for the KarlsenHashv2 mainnet,** providing enhanced performance, scalability, and security compared to its [Golang node](https://github.com/karlsen-network/karlsend).

We encourage developers and blockchain enthusiasts to collaborate, test, and optimize this Rust-based implementation. Your contributions will help shape a platform focused on scalability and speed without compromising decentralization.

## Compilation Requirements

To compile the binaries yourself (if you're unsure whether you need to, you probably don't), you'll need to install [Rust](https://www.rust-lang.org/tools/install).

## Setup

Mining Karlsen requires three components:

1. **Node (karlsend):** Listens for new blocks.
2. **Miner:** Searches for blocks to report to the node.
3. **Wallet:** For creating and managing wallets.

All three components are standalone executables that require no installation.

You can either download the precompiled binaries or compile the codebase yourself. For most users, downloading the binaries is recommended.

> **Note:** The karlsend node and miner must run concurrently, each from a separate console. Do not interrupt them while mining is in progress.

### Download Binaries

The simplest way to get started with Rusty-Karlsen is by downloading the binaries from [here](https://github.com/karlsen-network/rusty-karlsen/releases/latest). After downloading, extract them to a folder.

> **Important:** The rest of this guide assumes installation from source. Before running any command, navigate to the folder where you extracted the binaries:
```bash
$ cd <THE_EXTRACTED_BINARIES_FOLDER>
```

Linux and Mac users may need to prepend commands with `./` to execute the binary. For example:
```bash
./karlsend --utxoindex
```

### Build from Source

<details>
<summary>Building on Linux</summary>

1. Install general prerequisites:
    ```bash
    sudo apt install curl git build-essential libssl-dev pkg-config 
    ```

2. Install Protobuf (required for gRPC):
    ```bash
    sudo apt install protobuf-compiler libprotobuf-dev
    ```

3. Install the Clang toolchain (required for RocksDB and WASM secp256k1 builds):
    ```bash
    sudo apt-get install clang-format clang-tidy clang-tools clang clangd libc++-dev libc++1 libc++abi-dev libc++abi1 libclang-dev libclang1 liblldb-dev libllvm-ocaml-dev libomp-dev libomp5 lld lldb llvm-dev llvm-runtime llvm python3-clang
    ```

4. Install the [Rust toolchain](https://rustup.rs/).
   If Rust is already installed, update it with:
    ```bash
    rustup update
    ```

5. Clone the repository:
    ```bash
    git clone https://github.com/karlsen-network/rusty-karlsen
    cd rusty-karlsen
    ```

6. Build the binaries:
    ```bash
    cargo build --release --bin karlsend --bin karlsen-wallet
    ```
</details>

<details>
<summary>Building on Windows</summary>

1. [Install Git for Windows](https://gitforwindows.org/) or an alternative Git distribution.

2. Install [Protocol Buffers](https://github.com/protocolbuffers/protobuf/releases/download/v21.10/protoc-21.10-win64.zip) and add the `bin` directory to your `Path`.

3. Install [LLVM](https://github.com/llvm/llvm-project/releases/download/llvmorg-15.0.6/LLVM-15.0.6-win64.exe) and add the `bin` directory of the LLVM installation (`C:\Program Files\LLVM\bin`) to `PATH`.

   - Set the `LIBCLANG_PATH` environment variable to point to the `bin` directory as well.
   - Rename `LLVM_AR.exe` to `AR.exe` in the LLVM installation directory to address potential C++ dependency issues.

4. Install the [Rust toolchain](https://rustup.rs/).
   If Rust is already installed, update it with:
    ```bash
    rustup update
    ```

5. Clone the repository:
    ```bash
    git clone https://github.com/karlsen-network/rusty-karlsen
    cd rusty-karlsen
    ```

6. Build the binaries:
    ```bash
    cargo build --release --bin karlsend --bin karlsen-wallet
    ```
</details>

<details>
<summary>Building on macOS</summary>

1. Install Protobuf (required for gRPC):
    ```bash
    brew install protobuf
    ```

2. Install LLVM:
   - The default Xcode installation does not support WASM build targets. Install LLVM from Homebrew:
    ```bash
    brew install llvm
    ```

   - Add the following to your `~/.zshrc` file:
    ```bash
    export PATH="/opt/homebrew/opt/llvm/bin:$PATH"
    export LDFLAGS="-L/opt/homebrew/opt/llvm/lib"
    export CPPFLAGS="-I/opt/homebrew/opt/llvm/include"
    export AR=/opt/homebrew/opt/llvm/bin/llvm-ar
    ```

   - Reload the `~/.zshrc` file:
    ```bash
    source ~/.zshrc
    ```

3. Install the [Rust toolchain](https://rustup.rs/).
   If Rust is already installed, update it with:
    ```bash
    rustup update
    ```

4. Clone the repository:
    ```bash
    git clone https://github.com/karlsen-network/rusty-karlsen
    cd rusty-karlsen
    ```

5. Build the binaries:
    ```bash
    cargo build --release --bin karlsend --bin karlsen-wallet
    ```
</details>

## Getting Started

Karlsend offers various configuration options, but most basic operations require no configuration except for the `--utxoindex` flag (which can be omitted if you're not using the wallet):

```bash
$ karlsend --utxoindex
```

Use `karlsend --help` to view more running options.

### Creating a Wallet (Optional)

To mine, you'll need a keypair. Start by launching the wallet:

```bash
$ ./karlsen-wallet
```

On Windows:
```bash
$ karlsen-wallet.exe
```

Use the "help" command to explore available options. For instance, to create a wallet:

1. Select a Karlsen network:
    ```bash
    $ network mainnet
    ```

2. Create a wallet:
    ```bash
    $ wallet create
    ```

3. Request a new address:
    ```bash
    $ address new
    ```

Your screen will show you something like this:
```
Your default account deposit address:
karlsen:0123456789abcdef0123456789abcdef0123456789
```

**Note:** Each time you request an address, a new one will be generated. This is normal, as each secret key can correspond to multiple public addresses.

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

To make your node publicly accessible, forward port 42111 
(or the configured port) to the machine running karlsend. Public nodes help the overall health of the network by allowing others to sync with your node.

### rusty-karlsen Hardware Requirements

**Minimum:**
- 100 GB disk space
- 7th generation i7 4-core processor or AMD equivalent
- 8 GB memory
- 10 Mbit internet connection

**Recommended:**
- 9th generation i7 8-core processor or AMD equivalent
- 16 GB memory
- 40 Mbit internet connection

### Discord

[![Join the Karlsen Discord Server](https://img.shields.io/discord/1169939685280337930.svg?label=&logo=discord&logoColor=ffffff)](https://discord.gg/ZPZRvgMJDT)

You may want to join our discord server for further questions: https://discord.gg/ZPZRvgMJDT