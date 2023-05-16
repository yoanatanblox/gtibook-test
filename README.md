# Run a Distributed Validator

The SSV network enables the distribution of validator operations between non-trusting operators.

![](.gitbook/assets/0.png)

In order to distribute your validator, you must have an activated (deposited) validator on the Beacon Chain.

## How to Run a Distributed Validator <a href="#_qbxicu1vhvv3" id="_qbxicu1vhvv3"></a>

This guide outlines the steps required to run a distributed validator via the ssv.network, using the protocol’s smart contracts and developer tools.

### Prerequisites <a href="#_v2zo33nxl8mr" id="_v2zo33nxl8mr"></a>

* **An Ethereum validator** - generation of validator keys and their activation (deposit) to the Beacon Chain could be done using Ethereum’s official [Staking Deposit CLI](https://github.com/ethereum/staking-deposit-cli) and their [Launchpad](https://goerli.launchpad.ethereum.org/).
* **Goerli ETH** (<0.1) to cover transaction gas costs on the Goerli testnet (Community members on our [discord](https://discord.gg/D5kUdV93) could assist in obtaining the required amount).
* **Testnet SSV** ([faucet](https://faucet.ssv.network/))

### Process Overview <a href="#_7f2y4pcm8bfl" id="_7f2y4pcm8bfl"></a>

Validators are managed within Clusters - the group of operators that were selected to operate them.

Running a distributed validator is outlined by the following steps:

1. Select the group of operators to manage your validator.
2. Split your validator key to shares.
3. Retrieve your cluster’s latest snapshot data.
4. Register your validator to the network.

![Process Diagram](.gitbook/assets/1.png)

#### 1. Operator Selection <a href="#_tulnbjthau7t" id="_tulnbjthau7t"></a>

Select your preferred group of operators from the operator registry of the SSV network - 4 operators are required for each cluster.

For each chosen operator, you must fetch its network assigned **id** and its corresponding **key**.

The entire operator registry can be viewed via the ssv.network’s [explorer](https://explorer.ssv.network/operators) or through the SSV API to get access to the necessary operator **ids / keys**.

![Operator Page in ssv.network Explorer](.gitbook/assets/2.png)



#### 2. Split Validator Key to Shares <a href="#_x02jw9rs53s3" id="_x02jw9rs53s3"></a>

To assign the validator operation to the cluster of your selected operators, you must split your validator key to shares.

Use the [SSV Keys](tools/ssv-keys/ssv-keys-cli.md) tools to extract your validator key from your keystore file and split it to shares.



#### 3. Retrieve Cluster Snapshot Data <a href="#_vjco67d7q0gy" id="_vjco67d7q0gy"></a>

Cluster snapshots are required for SSV smart contract transactions with cluster related functions which require the **cluster object** as input.

Cluster snapshots are updated after each transaction with a cluster related function, and will emit a new **cluster object** (the latest snapshot) which will be required for making the succeeding transaction.

Use the [Cluster Scanner](tools/cluster-scanner/) tools to retrieve the latest snapshot data for your validator’s cluster.



#### 4. Network Registration <a href="#_j9fra6w5d8er" id="_j9fra6w5d8er"></a>

To signal your cluster to start operating your validator, you must register your validator to the network by broadcasting the [registerValidator()](smart-contracts-v3/ssvnetwork.md#public-registervalidator-publickey-operatorids-shares-amount-cluster) transaction to the ssv.network contract:

| Parameter   | Type      | Description                                                                                                                                                                                                                                                                                                                             |
| ----------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| publicKey   | bytes     | The validator’s public key.                                                                                                                                                                                                                                                                                                             |
| operatorIds | uint64\[] | List of all operator’s ids which were selected during the “Operators Selection” step.                                                                                                                                                                                                                                                   |
| shares      | bytes\[]  | Shares which were produced during the “Key Splitting” step.                                                                                                                                                                                                                                                                             |
| amount      | uint256   | Amount of SSV tokens to be deposited as payment (not mandatory). See [validator funding](https://docs.ssv.network/learn/stakers#validator-funding) to calculate how much funding is needed to run each validator.                                                                                                                       |
| cluster     | tuple\[]  | <p>Object containing the latest cluster snapshot data, produced during the “Retrieve Cluster Snapshot” step - obtained using the <a href="tools/cluster-scanner/">Cluster-Scanne</a>r tool.</p><p></p><p><strong>If this is the 1st validator within a specific cluster (unique set of operators), use - {0,0,0,0,0,false}</strong></p> |

You can construct the transaction by yourself or through using the partial payload generated by the [SSV Keys](tools/ssv-keys/) tool used in the “Key Splitting” step.

* Please note that as SSV is deposited to the contract, you must initially approve the SSV contract address to spend your SSV tokens prior to the registration transaction.
