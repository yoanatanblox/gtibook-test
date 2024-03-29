# Installation Guide

Once you have your node running you'll be able to participate in multiple validator clusters and earn rewards 🥳

### System Requirements

The SSV node that you are installing with these instructions is only the SSV node, not an Ethereum Execution Client or Beacon Client (e.g. not Get/Lighthouse or Besu/Teku, etc.). You will need those clients to already be running and synced, either on a different machine or the same machine.

All cloud services are supported for your node setup (see a reference example on AWS [here](https://github.com/bloxapp/ssv/blob/main/docs/OPERATOR\_GETTING\_STARTED.md#setting-aws-server-for-operator)).

The minimum system requirements shown below are for a machine that is only running an SSV node. If you plan to run the SSV node on the same machine as your Execution Client and/or Beacon Client, these minimum requirements will be needed **in addition** to your existing requirements.

💻 Machine running Ubuntu

🎛️ 4 cores (3 minimum)

⚡️ 4GB RAM

📀 20GB storage (5GB minimum)

🧮 IOPS > 10K

### Ethereum Node Requirements

The Ethereum clients used by your SSV node can be running on the same machine or a different machine. Ideally, to improve [client diversity](https://clientdiversity.org/#distribution) (Erigon client is still not supported), you will be running minority clients 👀 .

#### Execution Client

This can be any Ethereum Execution client (e.g. Geth, Besu, etc.). You will see this node referenced as ETH1 in the SSV configuration.

{% hint style="info" %}
You **MUST** enable WebSocket on your Execution Client as the SSV node requires that connection to work.

The specific configuration will be different for each Execution Client. For example, for Geth, add the command line flag `--ws` to the Geth start command to enable the WebSocket RPC server.
{% endhint %}

#### Beacon Client

This can be any Ethereum Beacon Node client (e.g. Prysm, Lighthouse, Tekou, Nimbus, or any client utilizing standard REST HTTP). You will see this node referenced as ETH2 in the SSV configuration.

### Installing the SSV Node

Connect to your server:

<details>

<summary>SSH into a local machine</summary>

[https://docs.ethstaker.cc/ethstaker-knowledge-base/tutorials/connect-via-ssh](https://docs.ethstaker.cc/ethstaker-knowledge-base/tutorials/connect-via-ssh)

</details>

<details>

<summary>SSH into a Cloud server (e.g. AWS)</summary>

If you have generated an SSH key for your server or downloaded one from your Cloud hosting provider (e.g. AWS)

**MacOS**

```
cd ./{path to the folder to which the key pair file was downloaded}

chmod 400 {key pair file name}

ssh -i {key pair file name} ubuntu@{instance public IP you took from AWS}

```

**Windows**

```
cd /{path to the folder to which the key pair file was downloaded}

ssh -i {key pair file name} ubuntu@{instance public IP you took from AWS}
```

</details>

Once you're connected and on the command line, the next step is to install the SSV node software.

{% hint style="success" %}
SSV node setup is also available using [eth-docker](https://eth-docker.net/Support/BloxSSV/) which can be used to run the execution and consensus clients as well.
{% endhint %}

### Installation Script

Type (or copy and paste) these commands into your terminal on your SSV node machine that you should now be connected to:

```bash
# Install dependencies.
sudo apt-get update -y
sudo apt-get install -y make curl git dnsutils wget

# Install yq.
sudo wget -O /usr/local/bin/yq https://github.com/mikefarah/yq/releases/download/3.3.0/yq_linux_amd64
sudo chmod +x /usr/local/bin/yq

# Install Docker.
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get -y update
sudo apt-get -y install docker-ce docker-ce-cli containerd.io
```

### Generate Operator Keys

Your Operator <mark style="color:green;">**Public Key (PK)**</mark> and <mark style="color:green;">**Secret Key (SK)**</mark> are generated with this command:

```bash
docker run -it --rm bloxstaking/ssv-node:latest /go/bin/ssvnode generate-operator-keys
```

{% hint style="info" %}
Your PK is required when registering your operator to the network.
{% endhint %}

The command output will look like this:

{% tabs %}
{% tab title="PK Example" %}
LS0tLS1CRUdJTiBSU0EgUFVCTElDIEtFWS0tLS0tCk1JSUJJakFOQmdrcWhraUc5dzBCQVFFRkFBT0NBUThBTUlJQkNnS0NBUUVBMDswYVdEK3RibndzYVdLYjF3UnEKM0xheW8rL1dSeGh3aVJ0aXFsL0dmZGozaHY0Unh5K1FwVzh6666RK1dJNmJ1VFc4bzN2ZmsydDMwNUlQRTdCVApZR3ZoS666MFNoYmlHVXVQcXpxQnVSTjB6OTUxV3VlcEJwV3RkeTdUaDVsT0w1cTQ3REFqbFFDdi95NlJLZzM5Ck9nTXZnZ1BaNTRNWHJZcFdINlJqa3hoVUxvWXQxTEVBN05pU3JHU3JqdGxCTlZiRHR5d666WFp0SnNkM2tjbTMKNkw0anZHd2I0RjhqTmlzSUU5eWFLd2J1SmV6dHpGdjY1YXRiV25hVFdzbmg1bDNrZ05uMlJLWktqZ1pycmRGdApuT2t3Vmh6M2JDRTFUZWpua1kwLzN4QTBIWjVONC9IUUF1Rit2TllYb040aDBicnVTdlVmZTBLTndvMDNFQ3l3Ckl3SURBUUFCCi0tLS0tRU5EIFJTQSBQVUJMSUMgS0VZLS0tLS0U
{% endtab %}

{% tab title="SK Example" %}
LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcEFJQkFBS0NBUUVBMCswYVdEK3RibndzYVdLYjF3UnEzTGF5bysvV1J4aHdpUnRpcWwvR2ZkajNodjRSCnh5K1FwVzhFRUNRK1dJNmJ1VFc4bzN8YmsydDMwNUlQRTdCVFlHdmhKem4wU2hiaUdVdVBxenFCdVJOMHo5NTEKV3VlcEJwV3RkeTdUaDVsT0w1cTQ3REFqbFFDdi95NlJLZzM5T2dNdmdnUFo2VE1YcllwV0g2UmpreGhVTG9ZcwpxTEVBN05pU3JHU3JqdGxCTlZiRHR5d3lGWFp0SnNkM6gykTM2TDRqdkd3YjRGOGpOaXNJRTl5YUt3YnVKZXq0RnpGdjY1YXRiV25hVFdzbmg1bDNrZ05uMlJLWktqZ1pycmRGdG5Pa3dWaHozYkNFMVRlam5rWTAvM3hBMEhaNU4KNC9IU666Rit2TllYb040aDBicnVTdlVmZTBLTndvMDNFQ3l3SXdJREFRQUJBb0lCQUQvbW9XZjBvMlhLR1ZZWgpmcVlCMWZzQk43SkkwaEtUNHZMa2lBYVpaRzl6NlljUnV1aVZoZ2JzQjR5RENSWWd3Z0hCbTBTc1NFamFRY0pRCnF5MGpvTEJWTndtdDV1UWtMRDYyVXZhdGFJb1d2TVVrN2J2Z1dFMzgrZFlURDRNMmphVzdBSUZ2TG50eVBwOHkKT21FMDRLTUtiTnZHTDRHcWZ6dzdseVpwV2dEeTY0bWdkMk8rd21aZFNhdkR0TGNza666bHVSTEQxYklKVDQxSApwY3hKVk5qVmhFU3NGM1NGdXM4ZmpERXJiYmFQbnNTNWI3Z0hGUUJpZG5iYWhjOG5MOGFkT2M5Nks4a2FIWEFYCjJlSEloQitwSmdwUjU0bXY1bjZWTFljTUhXVTVyWE14emNicXQxVGFuMjI3MTA2NTRRQmIzY1ByT1V5UkI3REkKU1NEUzc0a0NnWUVBN1U4M0t4cWJpQy9mR2c4VUxMVjAzWktrOCtHWlNKZFlhN2ZnS2dXVCsvQjJLa2xPY2Y0TAoxcHYxMmlFbTdnVE5FYnNhaElpTm8wYlo5eEx2aG9NdTZoNSp==
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Be careful when you select and copy the entire **Secret Key (SK)** and make sure to include any "=" characters at the very end 👀
{% endhint %}

{% hint style="danger" %}
Make sure to store and back up your operator's **Secret Key (SK)** in a safe place.

**Do not share** this key with anyone.
{% endhint %}

### Create Configuration File

Copy the following `config.yaml` file:

```
global:
  # Console output log level 
  LogLevel: info
  
  # Debug logs file path
  LogFilePath: ./data/debug.log

db:
  # Path to a persistent directory to store the node's database.
  Path: ./data/db

ssv:
  # The SSV network to join to
  # Mainnet = Network: mainnet (default)
  # Testnet = Network: jato-v2
  Network: mainnet
  
  ValidatorOptions:
    # Whether to enable MEV block production. Requires the connected Beacon node to be MEV-enabled.
    BuilderProposals: false

eth2:
  # HTTP URL of the Beacon node to connect to.
  BeaconNodeAddr: http://example.url:5052

eth1:
  # WebSocket URL of the Eth1 node to connect to.
  ETH1Addr: ws://example.url:8546/ws

p2p:
  # Optionally provide the external IP address of the node, if it cannot be automatically determined.
  # HostAddress: 192.168.1.1

  # Optionally override the default TCP & UDP ports of the node.
  # TcpPort: 13001
  # UdpPort: 12001

# Note: Operator private key can be generated with the `generate-operator-keys` command.
OperatorPrivateKey: <operator-private-key>

# This enables monitoring at the specified port, see https://github.com/bloxapp/ssv/tree/main/monitoring
MetricsAPIPort: 15000
```

{% hint style="warning" %}
Make sure your `ETH1Addr` endpoint is communicating **over WebSocket** and **not over HTTP** in order to support subscriptions and notifications.
{% endhint %}

### Create and Start the Node using Docker

Now, for the part you've been waiting for... actually starting your SSV node!

To start your node, run the following Docker command in the same folder you created the `config.yaml` file in the previous step:

```bash
docker run -d --restart unless-stopped --name ssv_node -e \
CONFIG_PATH=/config.yaml -p 13001:13001 -p 12001:12001/udp -p 15000:15000 \
-v "$(pwd)/config.yaml":/config.yaml -v "$(pwd)":/data -it \
"bloxstaking/ssv-node-unstable:latest" make BUILD_PATH="/go/bin/ssvnode" start-node && \ 
docker logs ssv_node --follow
```

### Docker FAQ

<details>

<summary><strong>What happens if my machine restarts?</strong></summary>

If you look at the Docker command you ran to start the node you'll see the part that says:

```bash
--restart unless-stopped
```

This means that the Docker container running your node will always try to restart if it crashes, or if the whole machine turns off and then restarts.

</details>

<details>

<summary><strong>How do I stop the node?</strong></summary>

In the command above, you named your node:

```bash
--name=ssv_node
```

This is how you will reference your node with other docker commands.

To stop the node run:

```bash
docker stop ssv_node
```

</details>

<details>

<summary>How do I start the node?</summary>

If for some reason your node has stopped (maybe you manually stopped it 🤔) you don't need to run the full creation command again, as that will actually throw an error saying that the Docker container already exists.

In the command above, you named your node:

```bash
--name=ssv_node
```

This is how you will reference your node with other docker commands.

To start a container, run the command:

```bash
docker start ssv_node
```

</details>

<details>

<summary>How do I view the logs again?</summary>

In the command above, you named your node:

```bash
--name=ssv_node
```

This is how you will reference your node with other docker commands.

To view the logs when your node is running, use the command:

```bash
docker logs ssv_node
```

</details>

<details>

<summary>How do I update my node?</summary>

In the command above, you named your node:

```bash
--name=ssv_node
```

This is how you will reference your node with other docker commands.

To update your SSV node, you will need to stop your current node:

```bash
docker stop ssv_node
```

Then remove it (this only removes the old Docker image, not all of your data!):

```bash
docker rm -f ssv_node
```

Then pull the latest image from SSV:

```bash
docker pull bloxstaking/ssv-node:latest
```

And finally... run the [creation command again from the top of this section](installation-guide.md#create-and-start-the-node-using-docker) to create a new Docker container with the latest SSV image.

</details>

### Other Configuration

When you set up your firewall on your SSV node machine, make sure to expose the ports that you set in the [container creation command](installation-guide.md#create-and-start-the-node-using-docker). The defaults are <mark style="color:green;">**12001 UDP**</mark> and <mark style="color:green;">**13001 TCP**</mark>.

If you don't want to use those default ports, it's best to change them in your `config.yaml` file as well as changing the ports on the container creation command. Simply changing the host port mappings on the Docker command isn't enough! You can also add your `HostAddress` to the config, which is the public static IP address of the machine.

```yaml
p2p:
  HostAddress: 206.22.63.189
  UdpPort: 12001
  TcpPort: 13001
```
