# OptionBlox Technical Whitepaper
## A Decentralized Derivatives Solution Built on Stellar

Markus Paulson-Luna: markuspaulsonluna@optionblox.com\
Andrew Pierskalla: andrew@optionblox.com\
Alexander Mootz: alex@optionblox.com

<p>&nbsp;</p>

### Abstract

This whitepaper covers the technical details of OptionBlox's decentralized derivative protocols. These details include explanations of the protocol tokens, accounts, and TSS(Turing Signing Server) txFunctions that makeup OptionBlox derivative protocols. This whitepaper will be continually updated as details regarding OptionBlox's core protocols change. It currently covers the covered and uncovered option protocols and the specifications of options contracts written using OptionBlox. The OptionBlox team plans to add details regarding our futures, swaps, and forwards protocols once those protocols get closer to launch.

<p>&nbsp;</p>

### Table of Contents:
- [Introduction](#introduction)
- [Contract Design](#Option-Contract-Design)
- [OptionBlox's Derivative Protocols](#txfunction-derivative-protocols)

<p>&nbsp;</p>

### Introduction:
The OptionBlox protocol serves as a tier-3 blockchain app, a layer between OptionBlox web-app users and the Stellar ledger. Users link their wallets to OptionBlox's web-app, which takes user inputs and communicates them to the OptionBlox protocol. The OptionBlox protocol uses TSS(Turing Signing Server) txFunctions to build Stellar transactions that write, execute, or cover derivatives for users. Users approve these transactions with their wallet and receive the result of their transaction, whether that’s derivative tokens, collateral deposits, or underlying assets.

#### High-Level Protocol Diagram

 ![alt text](https://raw.githubusercontent.com/markuspluna/OBXwhitepaper/master/photos/High-Level%20OBX%20protocol.png "OBX Protocol")

### Option Contract Design

#### Contract Terms
OptionBlox option contracts are all standard American style options, meaning they can be executed at any time until the expiration date. Currently, OptionBlox only offers covered option contracts, which means that the full underlying asset balance associated with an option contract is required to write an option. However, the OptionBlox team has an uncovered option protocol that will be rolled out in the future. This protocol would allow users to write options contracts by only providing a portion of the associated underlying asset. 

#### Contract Sizing
OptionBlox standardizes contract sizes by option type so that all options contracts for an asset pair have uniform underlying amounts. For example, all ETH-USD options will have an underlying of 1 ETH. This standardization helps maintain contract volume. The option token's issuing account stores contract sizes in its data entries. Contract sizes are also displayed on the OptionBlox web-app’s market page and available through OptionBlox's API endpoints. Contract sizes are set to keep strike prices within a 3 digit range for standard options and a 4 digit range for options with USD as their counter asset. Contract sizes can be adjusted if the underlying assets value changes and no longer allows for the desired strike price digit ranges.


### Derivative Tokens
OptionBlox tokenizes derivative contracts written with its protocol. Tokenization is critical to ensure that OptionBlox derivatives are tradeable on any cryptocurrency exchange, compatible with any wallet, and the efficacy of the execution txFunction.

#### Partial Contracts
Tokenizing derivatives allows OptionBlox users to write partial derivative contracts. Users can write options contracts with fractions of the contract underlying assets and will be sent an equivalent proportion of the derivative token for that contract. Partial contracts are crucial for allowing OptionBlox to maintain standardized contract sizes while allowing users with less capital to utilize the derivatives ecosystem.

To provide an example, say the contract size for OptionBlox ETH-USD options is 1 ETH. This means that the underlying associated with these contracts is 1 ETH, and users need to provide 1 ETH as collateral to write an ETH-USD option. If a user can only provide .5 ETH, OptionBlox’s partial derivative capability allows them to write half an option contract. They provide .5 ETH as collateral and receive .5 ETH-USD option tokens for them to sell. 


#### Option Token Naming Conventions:
The OptionBlox protocol uses 11 character names for its option tokens. The token names convey the assets involved in the option, the option's strike price, the option's expiration date, and whether the option is a put or call. More information, such as contract size, is stored in the token's issuing account.
##### Standard Option Token Naming Convention
For most options, the token names adhere to the following format:
- XXX(First, Second, and Third characters of the option token name)\
The last three numbers of the strike price. For example, if the contract strike price is 125, these characters are "125". Strike prices should not be more than three numbers and should not involve decimals. Contract size can be adjusted to ensure that these strike price character limits remain true.
- XX(Fourth and Fifth characters of the option token name)\
The first two characters of the underlying asset token name. For example, if the contract underlying is XLM, these characters are "XL".
- XX(Sixth and Seventh characters of the option token name)\
The first two characters of the counter asset token name. For example, if the counter asset is BTC, these characters are "BT".
- XX(Eighth and Ninth characters of the option token name)\
Month the option expires in. For example, if the option contract expires in July, these characters are "07".
- X(Tenth character of the option token name)\
Settlement period the option expires in. For example, if the option contract expires on July 1st, 2021, the first settlement period for outstanding July options, this character would be "0". Unfortunately, we cannot represent an actual day here due to character limits, so we represent the settlement period instead. The first settlement period of a month is the first date in the month where options expire; the second period is the second date where options expire, and so forth. Below is a table that exhibits this. Referring to the table, once options expiring on 07/01/21 expire, 07/01/23 options can be issued, their settlement period code will be 0.  

|Date:|07/01/21|07/07/21|07/14/21|07/21/21|07/28/21|07/01/22|07/07/22|07/14/22|07/21/22|07/28/22|
|---|---|---|---|---|---|---|---|---|---|---|
|Settlement Period:|0|1|2|3|4|5|6|7|8|9|
- X(Eleventh character of the option token name)\
P or C based on whether the option is a put or a call. For example, if the option is a put, this character is "P".

To bring it all together, the name of an XLM-EURT Call Option with a contract size of 1000 XLM, a strike price of 350, and an expiration date of 7/14/2021 would have the token name XLEU350072C

##### Naming Convention for Options with USD as the counter asset
Through the OptionBlox web-app, users are encouraged to write/buy options with USD as the counter asset. This concentrates market volume into asset pairs that include USD. Since options that have USD as a counter asset will be more common due to this measure, we use a slightly different naming convention for options with USD as the counter asset.
- XXXX(First four characters of the option token name)
The last four digits of the option contract's strike price. For example, if a contract’s strike price is 1245, these characters are "1245".
- XXX(Fifth, Sixth, and Seventh characters of the option token name)
The first three characters of the underlying asset token's name. For example, if the underlying asset token is ETH, these characters are "ETH". 
- XX(Eight and Ninth characters of the option token name)
Month the option expires in. For example, if the option contract expires in November, these characters are "11".
- X(Tenth character of the option token name)
The settlement period the option expires in for that month. For example, if the option contract expires on November 14th 2022, the seventh settlement period for outstanding November options, this character is "7". For a further explanation on settlement periods, see the section on standard option token names.
- X(Eleventh character of the option token name)
P or C based on whether the option is a put or call. For example, if the option is a call contract, this character is "C".

### txFunction Derivatives Protocols:
OptionBlox features a range of tradeable decentralized derivative products. These are enabled using TSS txFunctions. Their code is available on our [GitHub](https://github.com/optionblox/optionblox-contracts). This section provides a high-level description of OptionBlox’s derivative protocols.

#### Covered Options
A variety of protocol tokens, accounts, and txFunctions make up the OptionBlox covered options protocol.

##### Option Protocol Tokens
1. *Option Token*\
The option token represents ownership of the option contract. It is issued to the contract writer, and they can then sell it on the DEX or any other cryptocurrency exchange. The option buyer uses this token to execute the option contract.

2. *Option Exercise Token*/
The option exercise token is used by the protocol to execute the option contract. Users do not interact with the token of their own volition; it is purely a protocol token. The exercise token’s name is the same as the derivative token’s name.

3. *Underlying Asset Tokens*/
These are the tokens that represent the option contract's underlying asset. The contract writer provides them to write call options and the Write txFunction locks them in the writer's holding account. In the case of put options, the option owner/executor uses them to purchase the writer's counter asset.

4. *Counter Asset Tokens*/
These are the tokens that represent the option contract's counter asset. The contract writer provides them to write put options and the Write txFunction locks them in the writer's holding account. In the case of call options, the option owner/executor uses them to purchase the writer's counter asset.

##### Accounts Involved
1. *Writer Holding Account*\
This account holds the writer's underlying or counter asset tokens. It is controlled by the Write txFunction, the Execute txFunction, the Settle txFunction, the earlyClose txFunction, and the Close txFunction. The writer has no control over this account until the option contract either expires or is covered. 

2. *Option Token Issuing Account*\
This account issues the derivative token that represents ownership of options contracts. It also stores contract details in its data entries. The Write txFunction controls it.

3. *Exercise Token Issuing Account*/
This account issues the derivative exercise token used to execute options contracts. The Write txFunction and the Execute txFunction control it.

##### Covered Options txFunctions
1. *Write txFunction*\
Intakes contract parameters from the option contract writer, sets up the writer's holding account, and issues derivative tokens to the contract writer.
2. *Execute txFunction*\
Intakes execution request, derivative tokens, and the required underlying(for puts) or counter(for calls) asset tokens from the option contract owner. Then sends the underlying(for calls) or counter(for puts) asset tokens to the contract owner.
3. *Settle txFunction*\
Sends the proceeds from execution to the option contract writer and deletes the now void derivative and derivative exercise tokens.
4. *Close txFunction*\ 
Called after option contract expiration. Returns the committed underlying or counter asset tokens to the contract writer and deletes the derivative tokens, exercise tokens, and holding account. This contract also performs the Settle txFunction's operations if the Settle txFunction was not already called.
5. *earlyClose txFunction*\
Called when an option contract is exercised before the expiration date or when the contract writer covers their option by buying an identical option token. Returns the writer's committed underlying if the option was covered, deletes the now void derivative and derivative exercise tokens, and deletes the writer's holding account if the option was fully exercised or covered. This contract also performs the Settle txFunction's operations if the Settle txFunction was not already called.
##### Covered Options Protocol Diagraom
Below is a basic model showing the writing, sale, and exercise processes of a covered call with an underlying of 1 Bitcoin(BTC) and a strike price of 1000 Lumens(XLM).

![covered](./static/whitepaper/coveredOptionsDiagram.png "Covered Options")

##### Uncovered Options
OptionBlox uncovered options are similar to OptionBlox covered options. The key differences are that the holding account also serves as a margin account for the seller and the execution process differs slightly. This protocol uses the same accounts and tokens as the covered options protocol. The txFunctions used in this protocol perform the same functions as those involved in the covered call protocol, but their operations differ.

##### Uncovered Options Protocol Diagram
Below is a model showing the writing, sale, and execution process of an uncovered call. The call contract's underlying is 1 BTC, its strike price is 100 XLM, the initial margin requirement is 15%, and the minimum margin requirement is 10%.
![uncoverd](./static/whitepaper/uncoveredOptionsDiagram.png "Uncovered Options")

##### Futures
OptionBlox’s futures protocol also operates using protocol tokens, accounts, and TSS txFunctions. Instead of using derivative tokens, the futures protocol issues tokens for the underlying asset, so a party who enters a contract with an underlying of 100 XLM would receive 100 XLMFUTURE tokens. Contract parties exchange these tokens to enter futures contracts; the price they exchange tokens at represents the future contract's spot rate. OptionBlox settles futures daily using a [mark-to-market](https://www.cmegroup.com/education/courses/introduction-to-futures/mark-to-market.html] system. The txFunctions for this protocol are still under development. The OptionBlox team will add more details on the Futures protocol to this whitepaper as it gets closer to launch.

##### Forwards and Swaps
The OptionBlox team has internal forwards and swaps protocols we plan to implement in the future. They are modified versions of the OptionBlox futures protocol. We will add details regarding these protocols to this whitepaper once the protocol gets closer to launch

##### Liquidation Prodecures
OptionBlox uncovered options require position liquidation when the position holders become delinquent in meeting their margin requirements or fail to provide the necessary underlying to complete contract settlement. In these situations, the protocol attempts to liquidate positions on the Stellar DEX. However, if low volume makes this impossible, OptionBlox uses a TSS managed liquidity pool to liquidate the contracts. 

![uncoveredLqd](./static/whitepaper/uncoveredOptionsLiquidation.png "Insufficient Margin Liquidation (option)")

The OptionBlox liquidity pool is made up of user-provided funds and managed by TSS txFunctions. Users receive pool tokens in exchange for the funds they contribute. The tokens represent their pool contribution and govern the percentage of liquidation profits they receive. Because of the margin requirements for OptionBlox contracts, it will never be in the position holder's economic interest to allow liquidation, and there will always be an economic incentive to liquidate the position.
