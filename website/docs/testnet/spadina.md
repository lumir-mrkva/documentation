---
id: spadina
title: Spadina Eth2 launchpad onboarding
sidebar_label: Spadina Eth2 launchpad onboarding
---
This section outlines the step-by-step process for how to join the [Spadina multiclient testnet](https://spadina.launchpad.ethereum.org/) to run a Prysm eth2 beacon node and validator.

![image](https://i.imgur.com/3oUwf4N.png)

:::danger Ensure You Are Joining the Right Testnet
Spadina is a small testnet that serves a rehearsal for the eth2 genesis. If this isn't the testnet you wish to join, but instead participate in the larger Medalla testnet, follow our instructions [here](/docs/testnet/medalla).
:::

## Step 1: Get Prysm

To begin, follow the instructions to fetch and install Prysm for your operating system.

* [Using the Prysm installation script (Recommended)](/docs/install/install-with-script)
* [Using Docker](/docs/install/install-with-docker)
* [Building from source with Bazel (Advanced)](/docs/install/install-with-bazel)

## Step 2: Get Test ETH

To participate in eth2, you'll need to stake 32 ETH. For the Spadina eth2 testnet, we use test ETH from the Görli eth1 testnet. You can request this testnet ETH by joining our [discord server](https://discord.gg/prysmaticlabs).

## Step 3: Complete the onboarding process in the official eth2 launchpad

The [official eth2 launchpad](https://spadina.launchpad.ethereum.org/summary) is the easiest way to go through a step-by-step process to deposit your 32 ETH to become a validator. Throughout the process, you'll be asked to generate new validator credentials using the official Ethereum deposit command-line-tool [here](https://github.com/ethereum/eth2.0-deposit-cli). During the process, you will have generated a `validator_keys` folder under the `eth2.0-deposit-cli` directory. You can import all of your validator accounts into Prysm from that folder in the next step.

## Step 4: Import your validator accounts into Prysm

For this step, you'll need to copy the path to the `validator_keys` folder under the `eth2.0-deposit-cli` directory you created during the launchpad process. For example, if your eth2.0-deposit-cli installation is in your `$HOME` (or `%LOCALAPPDATA%` on Windows) directory, you can then run the following commands for your operating system

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs
  groupId="operating-systems"
  defaultValue="lin"
  values={[
    {label: 'Linux', value: 'lin'},
    {label: 'Windows', value: 'win'},
    {label: 'MacOS', value: 'mac'},
    {label: 'Arm64', value: 'arm'},
  ]
}>
<TabItem value="lin">

**Using the Prysm installation script**

```text
./prysm.sh validator accounts-v2 import --keys-dir=$HOME/eth2.0-deposit-cli/validator_keys --spadina
```

**Using Docker**

```text
docker run -it -v $HOME/eth2.0-deposit-cli/validator_keys:/keys \
  -v $HOME/Eth2Validators/prysm-wallet-v2:/wallet \
  -v $HOME/Eth2:/validatorDB \
  --name validator \
  gcr.io/prysmaticlabs/prysm/validator:latest \
  accounts-v2 import --keys-dir=/keys --wallet-dir=/wallet --datadir=/validatorDB --spadina
```

**Using Bazel**

```text
bazel run //validator:validator -- accounts-v2 import --keys-dir=$HOME/eth2.0-deposit-cli/validator_keys --spadina
```

</TabItem>
<TabItem value="win">

**Using the prysm.bat script**

```text
prysm.bat validator accounts-v2 import --keys-dir=%LOCALAPPDATA%\eth2.0-deposit-cli\validator_keys --spadina
```

**Using Docker**

```text
docker run -it -v %LOCALAPPDATA%\eth2.0-deposit-cli\validator_keys:/keys -v %LOCALAPPDATA%\Eth2Validators\prysm-wallet-v2:/wallet gcr.io/prysmaticlabs/prysm/validator:latest accounts-v2 import --keys-dir=/keys --wallet-dir=/wallet --spadina
```

</TabItem>
<TabItem value="mac">

**Using the Prysm installation script**

```text
./prysm.sh validator accounts-v2 import --keys-dir=$HOME/eth2.0-deposit-cli/validator_keys --spadina
```

**Using Docker**

```text
docker run -it -v $HOME/eth2.0-deposit-cli/validator_keys:/keys \
  -v $HOME/Eth2Validators/prysm-wallet-v2:/wallet \
  -v $HOME/Eth2:/validatorDB \
  --name validator \
  gcr.io/prysmaticlabs/prysm/validator:latest \
  accounts-v2 import --keys-dir=/keys --wallet-dir=/wallet --datadir=/validatorDB --spadina
```

**Using Bazel**

```text
bazel run //validator:validator -- accounts-v2 import --keys-dir=$HOME/eth2.0-deposit-cli/validator_keys --spadina
```

</TabItem>
<TabItem value="arm">

**Using the Prysm installation script**

```text
./prysm.sh validator accounts-v2 import --keys-dir=$HOME/eth2.0-deposit-cli/validator_keys --spadina
```

**Using Bazel**

```text
bazel run //validator:validator -- accounts-v2 import --keys-dir=$HOME/eth2.0-deposit-cli/validator_keys --spadina
```

</TabItem>
</Tabs>

## Step 5: Run your beacon node and validator

![image](https://i.imgur.com/3yH946I.png)

#### Beacon node
First, let's run the beacon node connected to the Spadina testnet. It will begin to sync with other nodes and will be ready for you to connect to it.  If your beacon node is still running from [Step 1](https://docs.prylabs.network/docs/testnet/spadina#step-1-get-prysm), you do not have to perform this portion of Step 5.  Skip to the [validator portion](#validator).

<Tabs
  groupId="operating-systems"
  defaultValue="lin"
  values={[
    {label: 'Linux', value: 'lin'},
    {label: 'Windows', value: 'win'},
    {label: 'MacOS', value: 'mac'},
    {label: 'Arm64', value: 'arm'},
  ]
}>
<TabItem value="lin">

**Using the Prysm installation script**

```text
./prysm.sh beacon-chain --spadina
```

**Using Docker**

```text
docker run -it -v $HOME/.eth2:/data -p 4000:4000 -p 13000:13000 -p 12000:12000/udp --name beacon-node \
  gcr.io/prysmaticlabs/prysm/beacon-chain:latest \
  --datadir=/data \
  --rpc-host=0.0.0.0 \
  --monitoring-host=0.0.0.0 \
  --spadina
```

**Using Bazel**

```text
bazel run //beacon-chain --spadina
```

</TabItem>
<TabItem value="win">

**Using the Prysm installation script**

```text
prysm.bat beacon-chain --spadina
```

**Using Docker**

1. You will need to share the local drive you wish to mount to to container \(e.g. C:\).
   1. Enter Docker settings \(right click the tray icon\)
   2. Click 'Shared Drives'
   3. Select a drive to share
   4. Click 'Apply'
2. You will next need to create a directory named `/prysm/` within your selected shared Drive. This folder will be used as a local data directory for [beacon node](/docs/how-prysm-works/beacon-node) chain data as well as account and keystore information required by the validator. Docker **will not** create this directory if it does not exist already. For the purposes of these instructions, it is assumed that `C:` is your prior-selected shared Drive.
3. To run the beacon node, issue the following command:

```text
docker run -it -v %LOCALAPPDATA%\Eth2:/data -p 4000:4000 -p 13000:13000 -p 12000:12000/udp gcr.io/prysmaticlabs/prysm/beacon-chain:latest --datadir=/data --rpc-host=0.0.0.0 --monitoring-host=0.0.0.0 --spadina
```

This will sync up the beacon node with the latest cannonical head block in the network. The Docker `-d` flag can be appended before the `-v` flag to launch the process in a detached terminal window.

</TabItem>
<TabItem value="mac">

**Using the Prysm installation script**

```text
./prysm.sh beacon-chain --spadina
```

**Using Docker**

```text
docker run -it -v $HOME/.eth2:/data -p 4000:4000 -p 13000:13000 -p 12000:12000/udp --name beacon-node \
  gcr.io/prysmaticlabs/prysm/beacon-chain:latest \
  --datadir=/data \
  --rpc-host=0.0.0.0 \
  --monitoring-host=0.0.0.0 \
  --spadina
```

**Using Bazel**

```text
bazel run //beacon-chain --spadina
```

</TabItem>
<TabItem value="arm">

**Using the Prysm installation script**

```text
./prysm.sh beacon-chain --spadina
```

**Using Bazel**

```text
bazel run //beacon-chain --spadina
```

</TabItem>
</Tabs>

:::tip Syncing your node
The beacon-chain node you are using should be **completely synced** before submitting your deposit. You may **incur minor inactivity balance penalties** if the validator is unable to perform its duties by the time the deposit is processed and activated by the ETH2 network. You do not need to worry about this if the chain has not started yet.
:::

#### Validator
Open a second terminal window. Depending on your platform, issue the appropriate command from the examples below to start the validator.

<Tabs
  groupId="operating-systems"
  defaultValue="lin"
  values={[
    {label: 'Linux', value: 'lin'},
    {label: 'Windows', value: 'win'},
    {label: 'MacOS', value: 'mac'},
    {label: 'Arm64', value: 'arm'},
  ]
}>
<TabItem value="lin">

**Using the Prysm installation script**

```text
./prysm.sh validator --spadina
```

**Using Docker**

```text
docker run -it -v $HOME/Eth2Validators/prysm-wallet-v2:/wallet \
  -v $HOME/Eth2:/validatorDB \
  --network="host" --name validator \
  gcr.io/prysmaticlabs/prysm/validator:latest \
  --beacon-rpc-provider=127.0.0.1:4000 \
  --wallet-dir=/wallet --datadir=/validatorDB \
  --spadina
```

**Using Bazel**

```text
bazel run //validator --spadina
```

</TabItem>
<TabItem value="win">

**Using the prysm.bat script**

```text
prysm.bat validator --spadina
```

**Using Docker**

```text
docker run -it -v %LOCALAPPDATA%\Eth2Validators\prysm-wallet-v2:/wallet -v %LOCALAPPDATA%\Eth2:/validatorDB --network="host" --name validator gcr.io/prysmaticlabs/prysm/validator:latest --beacon-rpc-provider=127.0.0.1:4000 --wallet-dir=/wallet --datadir=/validatorDB --spadina
```

</TabItem>
<TabItem value="mac">

**Using the Prysm installation script**

```text
./prysm.sh validator --spadina
```

**Using Docker**

```text
docker run -it -v $HOME/Eth2Validators/prysm-wallet-v2:/wallet \ 
  -v $HOME/Eth2:/validatorDB \
  --network="host" --name validator \
  gcr.io/prysmaticlabs/prysm/validator:latest \
  --beacon-rpc-provider=127.0.0.1:4000 \
  --wallet-dir=/wallet --datadir=/validatorDB \
  --spadina
```

**Using Bazel**

```text
bazel run //validator --spadina
```

</TabItem>
<TabItem value="arm">

**Using the Prysm installation script**

```text
./prysm.sh validator --spadina
```

**Using Bazel**

```text
bazel run //validator --spadina
```

</TabItem>
</Tabs>


## Step 6: Wait for your validator assignment

Please note that it may take from **5-12 hours** for nodes in the ETH2 network to process a deposit. In the meantime, leave both terminal windows open and running; once the node is activated by the ETH2 network, the validator will immediately begin receiving tasks and performing its responsibilities. If the chain has not yet started, it will be ready to start proposing blocks and signing votes as soon as the genesis time is reached.

To check on the status of your validator, we recommend checking out the popular block explorers: [beaconcha.in](https://spadina.beaconcha.in) by Bitfly.

![image](https://i.imgur.com/CDNc6Ft.png)

## Advanced wallet setups

For running an advanced wallet setups, our documentation includes comprehensive guides as to how to use the wallet built into Prysm to recover another wallet, use a remote signing server, and more. You can read more about it [here](https://docs.prylabs.network/docs/wallet/introduction).

**Congratulations, you are now fully participating in the Prysm ETH 2.0 Phase 0 testnet!**

**Still have questions?**  Stop by our [Discord](https://discord.gg/KSA7rPr) for further assistance!
