---
description: Using API3's dAPIs in your Smart Contracts
---

# API3

[API3](https://api3.org/) is a collaborative project to deliver traditional API services to smart contract platforms in a decentralized and trust-minimized way. It is governed by a decentralized autonomous organization (DAO), namely the [API3 DAO](https://api3.org/dao).

For more info about API3, check out their [docs](https://docs.api3.org/).

# Using dAPIs - API3 Datafeeds

[dAPIs](https://docs.api3.org/explore/dapis/what-are-dapis.html) are continuously updated streams of off-chain data, such as the latest cryptocurrency, stock and commodity prices. They can power various decentralized applications such as DeFi lending, synthetic assets, stablecoins, derivatives, NFTs and more.

The data feeds are continuously updated by [first-party oracles](https://docs.api3.org/explore/introduction/first-party.html) using signed data. dApp owners can read the on-chain value of any dAPI in realtime.

Due to being composed of first-party data feeds, dAPIs offer security, transparency, cost-efficiency and scalability in a turn-key package.

The [API3 Market](https://market.api3.org/dapis) enables users to connect to a dAPI and access the associated data feed services.

![](/src/SS4.png)

[*To know more about how dAPIs work, click here*](https://docs.api3.org/explore/dapis/what-are-dapis.html)

<!-- ## Types of dAPIs

### Self-funded dAPIs
Self-funded dAPIs offer developers the opportunity to experience data feeds with
minimal up-front commitment, providing a low-risk option prior to using a
managed dAPIs.

### Managed dAPIs
Managed dAPIs are sourced from multiple first-party oracles and aggregated using
a median function. Compared to self-funded dAPIs, **managed dAPIs are monetized**,
as API3 requires payment in USDC on Ethereum Mainnet to operate them. -->

*Follow these steps to use self-funded dAPIs in your smart contracts on the Fantom Network.*
## Subscribing to self-funded dAPIs

With self-funded dAPIs, you can fund the dAPI with your own funds. The amount of gas you supply will determine how long your dAPI will be available for use. If you run out of gas, you can fund the dAPI again to keep it available for use.

### **Exploring and selecting your dAPI**

The [API3 Market](https://market.api3.org/dapis) provides a list of all the dAPIs available across multiple chains including testnets. You can filter the list by chains and data providers. You can also search for a specific dAPI by name. Once selected you will land on the details page where you can find more information about the dAPI.

### **Funding a sponsor wallet**

Once you have selected your dAPI, you can activate it by using the [API3 Market](https://market.api3.org/) to send FTM to the `sponsorWallet`. Make sure your:

- Wallet is connected to the Market and is the same network as the dAPI you are funding.
- Balance of the wallet should be greater than the amount you are sending to the `sponsorWallet`.

![API3 Remix deploy](/src/SS1.png)

To fund the dAPI, you need to click on the **Fund Gas** button. Depending upon if a proxy contract is already deployed, you will see a different UI.

![API3 Remix deploy](/src/SS8.png)

Use the gas estimator to select how much gas is needed by the dAPI. Click on **Send FTM** to send the entered amount to the sponsor wallet of the respective dAPI.

![API3 Remix deploy](/src/SS2.png)
![API3 Remix deploy](/src/SS3.png)

Once the transaction is broadcasted & confirmed on the blockchain a transaction confirmation screen will appear.

![API3 Remix deploy](/src/SS5.png)

### **Deploying a proxy contract to access the dAPI**

Smart contracts can interact and read values from contracts that are already deployed on the blockchain. By deploying a proxy contract via the API3 Market, a dAPP can interact and read values from a dAPI like ETH/USD.

Note:

- *If a proxy is already deployed for a self-funded dAPI, the dApp can read the dAPI without having to deploy a proxy contract. They do this by using the address of the already deployed proxy contract which will be visible on the API3 Market.*


If you are deploying a proxy contract during the funding process, clicking on the **Get Proxy** button will initiate a transaction to your MetaMask that will deploy a proxy contract.

![API3 Remix deploy](/src/SS6.png)

Once the transaction is broadcasted & confirmed on the blockchain, the proxy contract address will be shown on the UI.

![API3 Remix deploy](/src/SS7.png)


## Reading from a self-funded dAPI

Here's an example of a basic contract that reads from a self-funded dAPI.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.17;

import "@openzeppelin/contracts/access/Ownable.sol";
import "@api3/contracts/v0.8/interfaces/IProxy.sol";

contract DataFeedReaderExample is Ownable {
    // This contract reads from a single proxy. Your contract can read from multiple proxies.
    address public proxy;

    // Updating the proxy address is a security-critical action. In this example, only
    // the owner is allowed to do so.
    function setProxy(address _proxy) public onlyOwner {
        proxy = _proxy;
    }

    function readDataFeed()
        external
        view
        returns (int224 value, uint256 timestamp)
    {
        (value, timestamp) = IProxy(proxy).read();
        // If you have any assumptions about `value` and `timestamp`, make sure
        // to validate them right after reading from the proxy.
    }
}
```

- `setProxy()` is used to set the address of the dAPI Proxy Contract.

- `readDataFeed()` is a view function that returns the latest price of the set dAPI.

You can read more about dAPIs [here](https://docs.api3.org/guides/dapis/subscribing-self-funded-dapis/). 

## [Try deploying it on Remix!](https://remix.ethereum.org/#url=https://gist.githubusercontent.com/vanshwassan/1ec4230956a78c73a00768180cba3649/raw/176b4a3781d55d6fb2d2ad380be0c26f412a7e3c/DapiReader.sol)

## Additional Resources

Here are some additional developer resources

- [API3 Docs](https://docs.api3.org/)
- [dAPI Docs](https://docs.api3.org/explore/dapis/what-are-dapis.html)
- [QRNG Docs](https://docs.api3.org/explore/qrng/)
- [Github](https://github.com/api3dao/)
- [Medium](https://medium.com/api3)
