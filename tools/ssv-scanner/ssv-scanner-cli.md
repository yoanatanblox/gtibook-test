# SSV Scanner CLI

The `SSV Scanner CLI` tool is a command-line interface that retrieves events data from the SSV network contract.

### Installation

Prerequisites _- this tool requires_ [_NodeJS_](https://nodejs.org/en/download/) _installed on your machine._

```bash
 1 git clone https://github.com/bloxapp/ssv-scanner.git
 2 cd ssv-scanner
 3 yarn
```

### Commands <a href="#_1rk5eeceo4ov" id="_1rk5eeceo4ov"></a>

<table data-header-hidden><thead><tr><th width="151"></th><th></th></tr></thead><tbody><tr><td><strong>Command</strong></td><td><strong>Description</strong></td></tr><tr><td><code>cluster</code></td><td>This command is used to retrieve the latest snapshot of a provided cluster from the SSV network contract.</td></tr><tr><td><code>nonce</code></td><td>This command is used to retrieve the validator registration nonce of a provided account from the SSV network contract.</td></tr></tbody></table>

### `cluster` Arguments

You can use **`yarn cli cluster --help`** to see all arguments and their descriptions.&#x20;

<table><thead><tr><th width="323">Argument</th><th width="85.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>--node-url (-n)</code></td><td>string</td><td>ETH1 (execution client) node endpoint URL</td></tr><tr><td>--<code>ssv-contract-address (-ca)</code></td><td>string</td><td>The SSV network contract address (<a href="https://docs.ssv.network/developers/smart-contracts/ssvnetwork">reference</a>)</td></tr><tr><td>--<code>owner-address (-oa)</code></td><td>int</td><td>The cluster owner address (in the SSV contract)</td></tr><tr><td>--<code>operator-ids (-oids)</code></td><td>string</td><td>Comma-separated list of operator IDs. The amount must be 3f+1 compatible.</td></tr></tbody></table>

**Run**

{% code overflow="wrap" %}
```bash
yarn cli cluster -n <ETH1_NODE_ENDPOINT_URL> -ca <SSV_CONTRACT_ADDRESS> -oa <CLUSTER_OWNER_ADDRESS> -oids <OPERATOR1_ID, OPERATOR2_ID, OPERATOR3_ID, OPERATOR4_ID>
```
{% endcode %}

**Output**

The cluster snapshot breakdown and the cluster object in transaction payload format.

**Example:**

```json
Cluster snapshot:
┌─────────────────┬────────────────────────┐
│     (index)     │         Values         │
├─────────────────┼────────────────────────┤
│ validatorCount  │          '1'           │
│ networkFeeIndex │          '0'           │
│      index      │      '4647545440'      │
│     active      │          true          │
│     balance     │ '11000000000000000000' │
└─────────────────┴────────────────────────┘
{
  "block": 9215770,
  "cluster snapshot": {
    "validatorCount": "1",
    "networkFeeIndex": "0",
    "index": "4647545440",
    "active": true,
    "balance": "11000000000000000000"
  },
  "cluster": [
    "1",
    "0",
    "4647545440",
    true,
    "11000000000000000000"
  ]
}
```

### `nonce` Arguments

You can use **`yarn cli nonce --help`** to see all arguments and their descriptions.&#x20;

<table><thead><tr><th width="323">Argument</th><th width="85.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>--node-url (-n)</code></td><td>string</td><td>ETH1 (execution client) node endpoint URL</td></tr><tr><td>--<code>ssv-contract-address (-ca)</code></td><td>string</td><td>The SSV network contract address (<a href="https://docs.ssv.network/developers/smart-contracts/ssvnetwork">reference</a>)</td></tr><tr><td>--<code>owner-address (-oa)</code></td><td>int</td><td>The account owner address (in the SSV contract)</td></tr></tbody></table>

**Run**

{% code overflow="wrap" %}
```bash
yarn cli nonce -n <ETH1_NODE_ENDPOINT_URL> -ca <SSV_CONTRACT_ADDRESS> -oa <ACCOUNT_OWNER_ADDRESS>
```
{% endcode %}

**Output**

The validator registration nonce of the provided owner (to be used in the next validator registration).

**Example:**

```json
Next nonce: 2  
```
