# Tagion SDK Core


This is back-end of the Tagion SDK. There are command-line utilities and the devnet program.

To quickly test the tagions transfer process on the devnet you just need to follow two guides:

1. [Run the Devnet Container](#run-the-devnet-container)
2. [Bootstrap Configuration (Easy Way)](#bootstrap-configuration-easy-way)

## Table of Contents
- [Tagion SDK Core](#tagion-sdk-core)
  - [Table of Contents](#table-of-contents)
  - [Directory Overview](#directory-overview)
- [Run the Devnet Container](#run-the-devnet-container)
  - [Prepare Your Environment](#prepare-your-environment)
  - [Prepare And Run the Container](#prepare-and-run-the-container)
    - [Step 1. Clone SDK Repository](#step-1-clone-sdk-repository)
    - [Step 2. Build the Docker Image](#step-2-build-the-docker-image)
    - [Step 3. Run the Docker Image](#step-3-run-the-docker-image)
    - [Step 4. Try CLI Tools](#step-4-try-cli-tools)
- [Bootstrap Configuration (Easy Way)](#bootstrap-configuration-easy-way)
- [Manual Devnet Configuration (Hard Way)](#manual-devnet-configuration-hard-way)
  - [Create Two Wallets](#create-two-wallets)
  - [Prepare the Devnet](#prepare-the-devnet)
  - [Start the Devnet](#start-the-devnet)
  - [Send Tagions Between Wallets](#send-tagions-between-wallets)
- [Conclusion](#conclusion)



## Directory Overview

``` bash
./devnet/ # CLI tool that starts Tagion Dev Network on local machine
./utils/
    ./dart/ # CLI tool to inspect DART database, and write to it directly.
    ./hibon/ # CLI tool to convert between `.hibon` and `.json` formats.
    ./tagion/ # CLI tool to create Tagion bills for devnet.
./wallet/ # CLI tool to send and receive Tagion bills
```

# Run the Devnet Container

## Prepare Your Environment

If you don't have git installed, please install it from [official website](https://git-scm.com/downloads). If you need video guidance:

- [How to install Git](https://www.youtube.com/watch?v=nbFwejIsHlY)
- [How to install Git (simplified)](https://www.youtube.com/watch?v=8dlIEMlPamg) by our community member, [Emil Vestergaard](https://forum.tagion.org/u/emil/summary)



If you don't have Docker installed, please follow official installation guides:

- [Docker for Windows](https://docs.docker.com/docker-for-windows/install/) - ([generic guide](https://www.youtube.com/watch?v=ymlWt1MqURY) or [simplified guide](https://www.youtube.com/watch?v=vr0TiIH6h6Y) by [Emil](https://forum.tagion.org/u/emil/summary))
- [Docker for Macos](https://docs.docker.com/docker-for-mac/install/) - ([guide](https://www.youtube.com/watch?v=MU8HUVlJTEY))
- [Docker for Linux](https://docs.docker.com/docker-for-mac/install/) - ([guide](https://www.youtube.com/watch?v=KCckWweNSrM))

## Prepare And Run the Container

Once you have Git and Docker installed on your machine, you can run the SDK container from where you will be able to interact with core utilities.

If you need a video guide, our community member, [Emil Vestergaard](https://forum.tagion.org/u/emil/summary), made a [video based on this guide](https://www.youtube.com/watch?v=kRzgp7o7kFU).

You will need [Git](https://git-scm.com/), [Git LFS](https://git-lfs.github.com/) and [Docker](https://www.docker.com/get-started).

### Step 1. Clone SDK Repository

``` bash
git clone https://github.com/tagion/sdk.git ./tagion-sdk
cd ./tagion-sdk
```

### Step 2. Build the Docker Image

``` bash
cd ./core
docker build -t tagion-sdk-core .
```

### Step 3. Run the Docker Image

``` bash
docker run -it tagion-sdk-core

# Or mount your project folder to container workspace,
# to keep the state of the container on your machine
cd ./my-project
docker run -it -v $PWD/:/workspace tagion-sdk-core
```

### Step 4. Try CLI Tools

Now you are in the devnet container. You can use all the utils, like that:

``` bash
hibonutil --help
tagionutil --help
```

You can run the devnet from here, and use the Wallet CLI.

# Bootstrap Configuration (Easy Way)

We prepared a quick configuration for you, so you can try wallet CLI, sending some tagions back and forth, without going through the whole process of preparing the devnet.

In the bootstrap setup we have 4 wallets in the directories from `w1` to `w4`, each one has the pin `1111`. The `w1` has some tagions on it, the other 3 do not. The directory `node0` has initial genesis DART database, with the Tagion bills (Tagion money) for the `w1`.

In this simple guide, we will transfer tagions from `w1` to `w2` and check the balances of each wallet.

**Let's begin!**

We will need two terminal windows for this guide - one for the devnet and the other for interacting with Wallet CLI. 

**In the first terminal**, run the Docker container that you have built in the previous step:

``` bash
# We are not mounting the volume this time,
# and we are naming the container for later use
docker run -it --name tagion tagion-sdk-core 
```

Now, use `bootstrap` script to get all the needed files in the `/workspace` directory and start the devnet.

``` bash
bootstrap # Will copy all the needed files
devnet # Will start the devnet
```

Ignore the error messages you see when starting the devnet, those are just informational logs.

Next, open another terminal session and ssh into the same container, where the devnet is running. Leave the devnet running in the first terminal.

**In the second terminal:**

``` bash
# We can use 'tagion' as a name, because 
# we named the container in the first step:
docker exec -it tagion bash 
```

Use this terminal to interact with Wallet CLI going further.

--- 

First, let's check the balance of the `w1`, it should have 50000 TGN.

``` bash
demo_balance w1
```

Now you are ready to move some tagions around.

Use `demo_send` script to send tagions from one waller to another:

``` bash
demo_send w1 w2 500 # We are sending 500 TGN from w1 to w2
```

You should get output like this:

```
0] b.owner        025a87708536aa5bec75d635bf162a6334e416f14334c251fb045b2525b6c5e3d6
0] account        025a87708536aa5bec75d635bf162a6334e416f14334c251fb045b2525b6c5e3d6
signed  true pkey=025a87708536aa5bec75d635bf162a6334e416f14334c251fb045b2525b6c5e3d6
```

If you look closely at the logs of the devnet that is running in another terminal session, you should notice the logs about this transfer. 

Under the hood, `w1` just generated a contract and sent it to one of the devnet nodes. The node then gossiped the transaction to other nodes and eventually, every node got the same graph of events, without any synchronization, thanks to Hashgrpah algorithm. Once the epoch in the Hashgraph was completed, each node updated its local DART file, creating a new Tagion bill, with public key, managed by `w2`. Now only `w2` can spend that new Tagion bill.

Wait for at least 30 seconds and check the balances:

``` bash
demo_balance
```

If you don't see the balance changed, wait for another 20-30 seconds and repeat the process. 

**Congratulations!** You just made the transaction on the devnet with very early versions of dev tools. If you'd like to get your hands even more dirty, follow the manual devnet configuration guide below.

# Manual Devnet Configuration (Hard Way)

If you want to go deep and configure the devnet manually, follow this guide. We will create two wallets, generate genesis file that puts some tagions on one of the wallets and will start the devnet. After that, you can use `tagionwallet` directly to create invoices and fulfill them via another wallet.

If you have completed the easy guide first, you need to start from clean slate this time.

## Create Two Wallets

Tagion does not have the account system, like most blockchains do. Instead, we store the bills in the database. The wallet software manages the keys for those bills. You can 'spend' the bills using the Tagion compatible wallet, in our case - Wallet CLI.

To send tagions from one wallet to another, the sender must know the public key of the receiver. Currently, we use primitive system of **invoices** for this. The receiver must generate the invoice, that will be fulfilled by the sender.

**Let's create some Wallets!**

First, we need to start `tagion-sdk-core` container:

``` bash
# If you want to keep the state of devnet,
# mount your local folder into the container:
cd ./my-project
docker run -it --name tagion -v $PWD/:/workspace tagion-sdk-core

# If not, just start the container
docker run -it --name tagion tagion-sdk-core

tagionwallet --help # Should output the help
```

Create two folders, one for each wallet. Let's name them `w1` and `w2` accordingly.

``` bash
mkdir w1 w2
```

Now we will go in the `tagionwallet` ~~kinda~~ GUI mode to create wallet configs. We need to repeat the process two times for each wallet.

``` bash
tagionwallet -g # Will enter the GUI mode
```

Press `c` to enter interactive mode and create the wallet. You will be asked to answer at least 3 security questions and enter a PIN code afterwards. With this security questions you can restore your wallet, in case you forgot the PIN code. (not yet implemented, so just type any answer).

Finally, your directory should look like this:

```
./w1/
    ./tagionwallet.hibon
./w2/
    ./tagionwallet.hibon
```

## Prepare the Devnet

To start the network we need to have the `.drt` file. It is the database file, that will be synced between the nodes and modified when the network runs.

By default, there is no tagions in the network, so we need to create a **genesis invoice** by one of the wallets and generate an initial  `.drt` file from it. 

```bash
cd ./w1
# Enter GUI mode and generate an invoice for any sum
tagionwallet -g 
# Enter Pincode
# Press 'a' to enter account
# Press 'i' to enter invoice mode
# Type invoice label, press enter and fill the sum of tagions to generate
# Quit the GUI by pressing ctrl+c

ls # Now you should see added invoice.hibon

# You can see the contents of this file by typing:
hibonutil ./invoice.hibon -p
```

Now, convert the invoice to the genesis file:

``` bash
cd ../ # Exit the w1 folder
pwd # Should output '/workspace'
tagionutil -i ./w1/invoice.hibon ./genesis.hibon
```

Generate a dart database based on the genesis file:

``` bash
dartutil --initialize --dartfilename dart.drt -i genesis.hibon --modify
```

Then, create a folder for the first node, and move the generated dart file there:

``` bash
mkdir node0
mv dart.drt ./node0/dart.drt
```

We need to manually move the `genesis.hibon` to the first wallet, and name it `bills.hibon`, this is how wallets keep track of the bills they manage.

``` bash
mv genesis.hibon w1/bills.hibon
```

Finally, your directory should look like this:

```
./genesis.hibon

./node0/
    ./dart.drt 
./w1/
    ./tagionwallet.hibon
    ./bills.hibon
./w2/
    ./tagionwallet.hibon
```

Perfect! Now everything is ready to start a devnet.

## Start the Devnet

Now, we have the `.drt` database file and we can start `tagionwave` that will spawn multiple nodes and sync the database between them. Once the devnet is started, we can send tagions between the wallets.

The devnet will create a folder for each node, and keep the `.drt` database file in each of them. But we need to create the first folder, so at least one node does have the database to begin with.

Open another terminal session and ssh into the container we created:

```bash
docker exec -it tagion bash # Will move you to the container
tagionwave --loops 0 # Will start the devnet
```

Ignore the error messages you see when starting the devnet, those are just informational logs.

Keep the devnet running in this second terminal session.

## Send Tagions Between Wallets

The network is running, good job! Now let's do what we came here to do - move some tagions around. You have two wallets. `w2` is empty, so let's transfer tagions from `w1`  to `w2`.

Create invoice from the `w2` (it will be fulfilled by the first wallet).

``` bash
cd ./w2
tagionwallet -g # Enter GUI to generate invoice
```

You need to enter the pin code of the second wallet, and press `i` to generate the invoice. Enter the invoice name (any string), amount and press enter, then quit the `tagionwallet` by pressing `ctrl + c` 

```bash 
ls # There should be invoice.hibon
```

Now let's pay the invoice:

``` bash
cd ../w1

# Replace <PIN> with the pin from w1
tagionwallet --update --pay ../w2/invoice.hibon --pin <PIN> -s
```

Under the hood, `w1` just generated a contract and sent it to one of the devnet nodes. The node then gossiped the transaction to other nodes and eventually, every node got the same graph of events, without any synchronization, thanks to Hashgrpah algorithm. Once the epoch in the Hashgraph was completed, each node updated its local DART file, creating a new Tagion bill, with the public key, managed by `w2`. Now only `w2` can spend that new Tagion bill.

Let's check the balance of both wallets now.

Wait for at least 30 seconds and check the balance of `w2`:

``` bash
cd ./w2
tagionwallet --update --ammount
```

If you don't see the balance changed, wait for another 20-30 seconds and repeat the process. You can do the same with `w1` to see the amount changed.

# Conclusion

Congratulations completing the guide! Yes, it wasn't easy. We are in the early days, but this release has a lot of core parts working together: Hashgraph, gossip protocol, DART synchronization, wallet with key management. Each next release will be more and more polished and user-friendly. 

New features are coming as well, so stay tuned and [leave a comment](https://forum.tagion.org/t/tagion-sdk-2020-3-release/28) on our forum. We want to hear your thoughts. Did you complete the guide? Was it too confusing or doable?


