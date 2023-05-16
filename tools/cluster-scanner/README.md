# Cluster Scanner

The Cluster Scanner tool enables users to retrieve cluster snapshots from the ssv.network contract.

Clusters are unique to each account and are defined by the combination of the validator’s **owner address** and **operator ids** which were selected to manage it during registration.

Cluster snapshots are required for SSV smart contract transactions with cluster related functions which require the **cluster object** as input:

* [registerValidator()](../../smart-contracts-v3/ssvnetwork.md#public-registervalidator-publickey-operatorids-shares-amount-cluster)
* [removeValidator()](../../smart-contracts-v3/ssvnetwork.md#public-removevalidator-publickey-operatorids-cluster)
* [deposit()](../../smart-contracts-v3/ssvnetwork.md#public-deposit-owner-operatorids-amount-cluster)
* [withdraw()](../../smart-contracts-v3/ssvnetwork.md#public-withdraw-operatorids-amount-cluster)
* [getBalance()](../../smart-contracts-v3/ssvnetworkviews.md#public-getbalance-owner-operatorids-cluster)
* [reactive()](../../smart-contracts-v3/ssvnetwork.md#public-reactivate-operatorids-amount-cluster)
* [isLiquidated()](../../smart-contracts-v3/ssvnetworkviews.md#public-isliquidated-owner-operatorids-cluster)
* [isLiquidatable()](../../smart-contracts-v3/ssvnetworkviews.md#public-isliquidatable-owner-operatorids-cluster)
* [liquidate()](../../smart-contracts-v3/ssvnetwork.md#public-liquidate-owner-operatorids-cluster)

Cluster snapshots are updated after each transaction with a cluster related function, and will emit a new cluster object (the latest snapshot) which will be required for making the succeeding transaction.

The Cluster Scanner tool returns only the **latest snapshot** for a provided cluster.&#x20;

Please note that alternatively, instead of using this tool, you could save the **cluster object** emitted in the event after each transaction, and use it as the input for the succeeding cluster related contract transaction.\
