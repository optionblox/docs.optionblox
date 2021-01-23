# Stellar - The ideal platform for OptionBlox
OptionBlox uses Stellar's decentralized ledger to support its protocol.

In Stellar’s words:  

> “Fundamentally, Stellar is a system for tracking ownership. It uses an accounting ledger, shared across a network of independent computers, to store two important things for every account holder: what they own (their account balances) and what they want to do with what they own (operations on those balances, like buy or sell offers).\
The computers that run Stellar and publish the ledger are called nodes. They systematically validate the ledger’s contents so it’s always consistent across the network. For example, when you send someone a dollar on a Stellar-built app, the nodes check that the correct balances were debited and credited, and each node makes sure every other node sees and agrees to the transaction.”

Stellar's open network allows anyone to submit a transaction to change the ledger. However, the transaction only changes the ledger after the nodes agree it is valid. In OptionBlox's case, before a derivative is exercised or traded, the local network of nodes agree upon the validity of the transaction. This prevents parties with ill intentions from engaging in illicit behavior on the network. Therefore, Stellar provides a secure financial network with one source of truth and can be instantly accessed and modified by anyone in the world.  

For more general information visit: [Stellar's Website](https://www.stellar.org/developers/guides/walkthroughs/stellar-smart-contracts.html).  
For more information on how the nodes validate transactions see: [Stellar Consensus Protocol](https://www.stellar.org/developers/guides/concepts/scp.html).  
For more information on how OptionBlox interacts with Stellar’s network visit: [Network Overview](https://www.stellar.org/developers/guides/get-started/index.html).

## Why we built on Stellar
Stellar’s focus on financial applications has made it the ideal network to develop OptionBlox on.\
Characteristics of the Stellar network crucial to OptionBlox:
- *Efficiency*:  
Stellar’s network is efficient, having both fast transaction times and low fees. This is essential for a derivative market and is one of the main areas where Stellar shines versus other networks like Ethereum.  
- *Decentralization*:  
Stellar's network is fully decentralized. There is no governing body OptionBlox needs to rely on; as long as Stellar has users, OptionBlox will function.
- *[Anchors](https://www.stellar.org/developers/guides/concepts/assets.html)*  
Stellar has a multi-asset functionality called anchoring that enables users to create custom assets on its ledger and tie them to real-world assets. OptionBlox uses this to tokenize derivatives and to write derivatives involving any asset.  
- *Flexible Transaction System*:  
Stellar has a flexible transaction system that allows us to create derivatives and make them trust-free, tradeable, and safe.  
- *[Turing Signing Servers]( https://tss.stellar.org/)*:\
The Stellar Community is currently discussing implementing a new ecosystem feature called Turing Signing Servers. These are a decentralized network of servers that hold uploaded smart contracts, called txFunctions, which the server ties to private keys. The servers sign transactions with these private keys if the transactions meet the specifications of the contracts. OptionBlox uses this tool to manage some of the Turing complete logic surrounding derivative contract processing. However, if the Stellar ecosystem does not implement this feature, OptionBlox will pivot to using an open-source repository to manage derivative contract logic.

## Security:

OptionBlox uses a variety of Stellar's features to ensure that derivatives and margin accounts associated with the protocol are secure.

- *Turing Signing Server Protocol*:\
The TSS protocol provides OptionBlox with a method of adding Turing complete smart contract logic to transactions without requiring our organization to control the accounts involved in the transactions. This further decentralizes OptionBlox without reducing efficiency.\
[More Info](https://github.com/tyvdh/turing-signing-server)

- *Multi-Sig*:\
In most OptionBlox protocols, derivative contract writers must store contract collateral holding accounts, which must be fully controlled by the OptionBlox protocol. Stellar’s multi-sig capability allows OptionBlox to add all necessary TSS txFunctions as signers on the holding account and remove all other signers, preventing the account from submitting transactions without using txFunctions. This security measure locks funds in collateral holding accounts until contract exercise or expiration.  
[More Info](https://www.stellar.org/developers/guides/concepts/multi-sig.html)

- *Timebound Transactions*:  
In all OptionBlox protocols, timebound transactions are used in the closing process to ensure certain transactions cannot be submitted early. Submitting some transactions early would disrupt the exercise process.  
[More Info](https://www.stellar.org/developers/guides/concepts/multi-sig.html)

- *Stellar Consensus Protocol*  
Stellar's consensus protocol rejects transactions when they do not align with the correct ledger state. For example, a user could not fill a sell offer if their account lacked the necessary funds to complete the trade.  
[More Info](https://www.stellar.org/developers/guides/concepts/scp.html)  

- *Account Flags*  
OptionBlox uses the authorization required and revokable flags on the derivative exercise token issuing account. This ensures that exercise tokens are only usable during the exercise process.
[More Info](https://www.stellar.org/developers/guides/concepts/accounts.html#flags)

- *SEP-0007 Integration*:\
The OptionBlox web-app uses Stellar's SEP-0007 protocol to send transaction envelopes to users who can then add their signature in a trusted wallet or exchange. This ensures that the OptionBlox app will never have to serve as custodian over user funds or keys.\
[More Info](https://github.com/stellar/stellar-protocol/blob/master/ecosystem/sep-0007.md)

<p>&nbsp;</p>

## Anchors:

The functionality of OptionBlox is tied directly to the availability of Anchors in the Stellar ecosystem. To use a non-native asset as a derivative contract’s underlying asset, a third party must already be anchoring the asset.  
Here are some non-native assets that are currently anchored by reputable parties:
- USD: US dollar anchor provided by [AnchorUSD](https://www.anchorusd.com/), a Stellar partner.
- XCN: Chinese yuan anchor provided by [FChain](https://fchain.io).  
- BTC: Bitcoin anchor provided by [Papaya](https://apay.io/in).
- EURT: Euro anchor provided by [Tempo](https://tempo.eu.com/en).
- NGNT: Nigerian Naira anchor provided by [Cowrie](https://www.cowrie.exchange/).
- GOLD: Gold anchor provided by [StellarMetals](stellarmetals.org).
- ETH: Etherium anchor provided by [Papaya](https://apay.io/in).
- DSTOQ: Equities anchored on Stellar by [DSTOQ](https://dstoq.com).

More anchors can be viewed on [Steller Expert]( https://stellar.expert/explorer/public/

As Stellar's network grows, we are confident that more anchors will materialize and expand OptionBlox's functionality.


