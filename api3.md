---
description: Using API3's dAPIs in your Smart Contracts
---

# API3

[API3](https://api3.org/) is a collaborative project to deliver traditional API services to smart contract platforms in a decentralized and trust-minimized way. It is governed by a decentralized autonomous organization (DAO), namely the [API3 DAO](https://api3.org/dao).

For more info about API3, check out their [docs](https://docs.api3.org/).

# Using dAPIs in your Smart Contracts

[dAPIs](https://docs.api3.org/dapis/) are continuously updated streams of off-chain data, such as the latest cryptocurrency, stock and commodity prices. They can power various decentralized applications such as DeFi lending, synthetic assets, stablecoins, derivatives, NFTs and more.

Due to being composed of first-party data feeds, dAPIs offer security, transparency, cost-efficiency and scalability in a turn-key package.

Follow these steps to use self-funded dAPIs in your smart contracts on the Fantom Network.

![API3 Remix deploy](/img/tools/api3/SS4.png)

## Subscribing to self-funded dAPIs

With Self-Funded dAPIs, you can fund the dAPI with your own funds. The amount of gas you supply will determine how long your dAPI will be available for use. If you run out of gas, you can fund the dAPI again to keep it available for use.

### **Exploring and selecting your dAPI**

The [API3 Market](https://market.api3.org/dapis) provides a list of all the dAPIs available across multiple chains including testnets. You can filter the list by chains and data providers. You can also search for a specific dAPI by name. Once selected you will land on the details page where you can find more information about the dAPI.

### **Funding a sponsor wallet**

Once you have selected your dAPI, you can fund it by using the Market to send funds to the `sponsorWallet`, make sure your:

- Wallet is connected to the Market and is the same network as the dAPI you are funding.
- Balance of the wallet should be greater than the amount you are sending to the `sponsorWallet`.

![API3 Remix deploy](/img/tools/api3/SS1.png)

To fund the dAPI, you need to click on the **Fund sponsor wallet/Fund Gas** button. Depending upon if a proxy contract is already deployed, you will see a different UI.

![API3 Remix deploy](/img/tools/api3/SS2.png)

Use the gas estimator to select how much gas is needed by the dAPI. Click on **Send FTM** to send the entered amount to the sponsor wallet of the respective dAPI.

![API3 Remix deploy](/img/tools/api3/SS3.png)

Once the transaction is broadcasted & confirmed on the blockchain a transaction confirmation screen will appear.

![API3 Remix deploy](/img/tools/api3/SS5.png)

### **Deploying a proxy contract to access the dAPI**

Smart contracts can interact and read values from contracts that are already deployed on the blockchain. By deploying a proxy contract via the API3 market, a dAPP can interact and read values from a dAPI like ETH/USD.

Note:

- *If a proxy is already deployed for a self-funded dAPI, the dAPP can read the dAPI without having to deploy a proxy contract by using the address of the already deployed proxy contract which will be visible on the API3 market.*


If you are deploying a proxy contract during the funding process, clicking on the **Deploy proxy** button will initiate a transaction to your Metamask that will deploy a proxy contract.

![API3 Remix deploy](/img/tools/api3/SS6.png)

Once the transaction is broadcasted & confirmed on the blockchain, the proxy contract address will be shown on the UI.

![API3 Remix deploy](/img/tools/api3/SS7.png)


## Reading from a self-funded dAPI

Here's an example of a basic contract that reads from a self-funded dAPI.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@api3/airnode-protocol-v1/contracts/dapis/proxies/interfaces/IDapiProxy.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract Reader is Ownable {
    address dapiProxy;

    function setDapiProxyAddress(address _proxyAddress) public onlyOwner {
        dapiProxy = _proxyAddress;
    }

    function readDapi() public view returns (int224 value, uint32 timestamp){
        return IDapiProxy(dapiProxy).read();
    }
}
```

- `setDapiProxyAddress()` is used to set the address of the dAPI Proxy Contract.

- `readDapi()` is a view function that returns the latest price of the set dAPI.

You can read more about dAPIs [here](https://docs.api3.org/dapis/). 

## [Try deploying it on Remix!](https://remix.ethereum.org/#url=https://gist.githubusercontent.com/vanshwassan/1ec4230956a78c73a00768180cba3649/raw/caff497e5b4b61d89d920b49da70779a0a24ac58/DapiReader.sol)
