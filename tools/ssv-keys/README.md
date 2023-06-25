# SSV Keys

The SSV Keys tools enable users to split a validator key into a predefined threshold of shares via [Shamir-Secret-Sharing (SSS)](https://en.wikipedia.org/wiki/Shamir's\_Secret\_Sharing), and encrypts them with a set of operator keys.

In addition to the generation of shares, the tool also uses the validator key to sign the validator’s owner address and his registration nonce to ensure that only the legitimate owner of the validator can register it to the network.

The shares and the signature are constructed as **sharesData** which is used during [validator registration](https://docs.ssv.network/developers/smart-contracts/ssvnetwork#public-registervalidator-publickey-operatorids-shares-amount-cluster) through the SSV smart contract in order to facilitate their distribution from stakers to operators.
