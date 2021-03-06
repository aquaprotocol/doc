# AquaProtocol

#### Note
The AquaProtocol is a project dedicated only to the Chainlink Virtual Hackathon 2021. And there is nothing to do with real projects with similar names.

### Description
The AquaProtocol is the decentralized investment platform where potential customers can deposit assets in a fiat currency or a cryptocurrency and then invest in the art, papers, or buy insurance. Purchases are possible in euro. For a money exchange between euro and national currency works the Chainlink oracle.
During the fiat, deposition is used an oracle with the Open Banking API. Smart contracts were installed in The Kovan Testnet.
The Core of the protocol is inspired by the AAVE solution.

### Important aspect:
- Easy access for a new potential customer to the blockchain world;
- Fast and safe exchange between fiat to a cryptocurrency or a stable coin and revert;
- Possibility of investment to a paper and art as the NFT;
- Possibility of buying insurance.
- The World State is recorded parallel in blockchain and a database - fast access to the recorded data.

### Main Components:

![The Picture shows AquaProtocol's main components](https://user-images.githubusercontent.com/81238266/113659154-ba093a00-96a1-11eb-9c82-7a8f60efc9bf.png)

The infrastructure of AquaProtocol is based on microservice architecture. The webpage is writing in React, smart contracts with Solidity, and backend in the GO.
The GO is used as the main backend language after the comprehensive performance test.
#### [Web](https://github.com/aquaprotocol/web)
The Webpage with the main dapp's functions. Like deposit/withdraw and possibly invest in the Art, the Paper and the Insurance.

#### [Smart Contract API](https://github.com/aquaprotocol/smart-contract-api)

The connector between frontend - backend. This component serves access to blockchain's smart contract functions and world state changer.

####  [Smart Contracts](https://github.com/aquaprotocol/smart-contract)

Blockchain's smart contracts. The main functions of the AquaProtocol.

####  [Oracle API](https://github.com/aquaprotocol/oracle-api)
The custom Oracle component. The Smart contract event listener invokes Open Banking API functions and then smart contracts callback functions.

#### [Open Banking API](https://github.com/aquaprotocol/open-banking-api)
The European Union's PSD2 directive allows third parties to access bank functions. The Open Banking API is a response to these problems. It describes Payment Initiation Service (PIS), Account Information Service (AIS), and Confirmation of the Availability of Funds (CAF). In the AquaProtocol now only domestic payment is implemented in fiat currency deposition and withdrawal. This component is not the only API it is also mock bank account functions that are invoked during the change owner procedure. Thanks to this approach, the testing process is not dependent on Bank Access.

### Main Functions:
In this part are short descriptions of the main AquaProtocol functions and flow of the processes.

#### Deposti/Withdraw

![obraz](https://user-images.githubusercontent.com/81238266/113665307-88966b80-96ad-11eb-8af0-0d63d78206e6.png)

For a description of the main AquaProtocol component please see the Main Component part. Deposit/Withdraw is tailed on fiat or crypto/stable currency manipulation possibility.

- The Fiat currency
1. Potential users deposit/withdraw their funds in national currency.
2. The Request goes to the backend. The DepositFiat/WithdrawFiat function invokes the AquaProtocol blockchain functions.
3. The Equivalent Treasury token is mining and then a specific amount is deposited on a PLCurrency contract.
4. On the AquaProtocol smart contract a BankFunctionAPI is running.
5. The BankFunctionOracle smart contract emits the InvokeFiatDepositEvent.
6. The InvokeFiatDepositEvent is listening on the OracleAPI and then invokes OpenBanking's domestic payment function.
7. The Transaction is emitting on the Blockchain.
8. Status of the transaction is obtained from the Kovan Etherscan API.

- The Cryptocurrency/Stablecoin
1. Potential users deposit/withdraw their funds.
2. The Request goes to the backend. The Deposit/Withdraw function invokes the AquaProtocol blockchain functions.
3. The Equivalent Treasury (ERC-20) token is mining and then a specific amount is deposited on the PLCurrency (ERC-20) contract.
4. Status of the transaction is obtained from the Kovan Etherscan API.

#### The Investment

The base functions of the AquaProtocol are an investment in the art, the paper, and the possibility of buying insurance.
For these three types of investment, the possibility flow is the same. Thus at the bottom of the picture, the OBJECT is a synonym of the investment possibilities.

![obraz](https://user-images.githubusercontent.com/81238266/113668380-71a64800-96b2-11eb-93b5-57be2d590032.png)

1. Potential users invest their funds in the Treasury (ERC-20) token in Art, Paper or buy insurance (ERC-721).
2. In the backend-specific functions are invoked.
3. The World State is updated.
4. In the AquaProtocol contract specific amounts of the Treasure token are swapped between the user account and the contract creator maker (operator).
5. In this POC example price of the investment possibility is in the EURO currency. The Chainlink provider for the GET requests is used to convert the national currency to the EURO.
6. ERC-721 tokens are created.
7. The status of the transaction is obtained from Kovan Etherscan API.

### Future:
- Now the AquaProtocol's smart contract interface has the possibility to implement all ERC 20, ERC 721 tokens, future plans assume to implement another smart contracts interface.
- Extension of the Open Banking API oracle on international payment.
- Adding WebSocket in place of the Kovan Etherscan API for direct integration between blockchain events and Webpage.

### Potential implementation:
- Standard bank
- Blockchain’s swap protocols as the AAVE and the UniSwap.
