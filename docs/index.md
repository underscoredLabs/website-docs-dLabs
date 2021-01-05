---
title: uToken
summary: A plugin to bridge Unity to Ethereum Tokens.
authors:
    - Leon Do 
---

# uToken Documentation

A plugin to bridge Unity to Ethereum Tokens.

## Introduction

[![Watch the video](https://user-images.githubusercontent.com/19412160/103559601-66aa0480-4e84-11eb-803a-6f854e93640a.png)](https://www.youtube.com/watch?v=_XnB1BjUSvQ)

## Ethereum

### Connect

```c#
string network = "mainnet"; // mainnet ropsten kovan rinkeby goerli
Ethereum ethereum = new Ethereum(network);
```

### Block Height

```c#
BigInteger blockHeight = await ethereum.BlockHeight();
print(blockHeight);
```

### Balance Of

```c#
string account  = "0xaca9b6d9b1636d99156bb12825c75de1e5a58870";
BigInteger wei = await ethereum.BalanceOf(account);
print("wei: " + wei);
print("eth: " + Web3.Convert.FromWei(wei));
```

### Send

If WalletScene is not being used, then a private key is required.

```c#
string privateKey = ""; // remove if using WalletScene
Ethereum ethereum = new Ethereum(network, privateKey);

string toAccount = "0x72b8Df71072E38E8548F9565A322B04b9C752932";
decimal ethAmount = 0.01m;

string transactionHash = await ethereum.Send(toAccount, ethAmount);
print(transactionHash);
```
## ERC1155

ERC-1155 contracts have two additonal fields.

- Contract address: The address that stores the balances of the token.

- Token Id: The specific token inside of the contract address.

For example. The contract address `0x5e30b1d6f920364c847512e2528efdadf72a97a9` can store all digital chess pieces. A pawn can be token id `17`.

### Connect

```c#
string network = "mainnet"; // mainnet ropsten kovan rinkeby goerli
string contract = "0x5e30b1d6f920364c847512e2528efdadf72a97a9";
ERC1155 erc1155 = new ERC1155(network, contract);
```

### Balance Of

```c#
string account = "0xaca9b6d9b1636d99156bb12825c75de1e5a58870";
string tokenId = "17";

BigInteger balance = await erc1155.BalanceOf(account, tokenId);
print(balance)
```

### Balance Of Batch

Balance of batch will get the balance of a list of accounts and token ids. For example:

Get the balance of account `0xaCA9B6D9B1636D99156bB12825c75De1E5a58870` with token id `17` and

balance of account `0xaCA9B6D9B1636D99156bB12825c75De1E5a58870` with token id `22`

```c#
string[] accounts = { "0xaCA9B6D9B1636D99156bB12825c75De1E5a58870", "0xaCA9B6D9B1636D99156bB12825c75De1E5a58870" };
string[] tokenIds = { "17", "22" };

List<BigInteger> batchBalances = await erc1155.BalanceOfBatch(accounts, tokenIds);

foreach (var balance in batchBalances) 
{
  print(balance);
};
```

### Safe Transfer From

```c#
string privateKey = ""; // remove if using WalletScene
ERC1155 erc1155 = new ERC1155(network, contract, privateKey);

string fromAccount = "0xaCA9B6D9B1636D99156bB12825c75De1E5a58870";
string toAccount = "0x72b8Df71072E38E8548F9565A322B04b9C752932";
string tokenId = "17";
string tokenAmount = "1";
string data = "0x0";

string transactionHash = await erc1155.SafeTransferFrom(fromAccount, toAccount, tokenId, tokenAmount, data);
print(transactionHash);
```

### Safe Batch Transfer From

Safe batch transfer from will send a list of transactions. For example:

Send `1` token with id `17` and

Send `2` tokens with id `22`.


```c#
string fromAccount = "0xaca9b6d9b1636d99156bb12825c75de1e5a58870";
string toAccount = "0x72b8Df71072E38E8548F9565A322B04b9C752932";
string[] tokenIds = {"17", "22"};
string[] tokenAmounts = {"1", "2"};
string data = "0x0";

string transactionHash = await erc1155.SafeBatchTransferFrom(fromAccount, toAccount, tokenIds, tokenAmounts, data);
print(transactionHash);
```

### URI

Returns meta data about the token.

```c#
string tokenId = "17";
string uri = await erc1155.URI(tokenId);
print(uri);
```

### Is Approved For All

Queries the approval status of an operator for a given owner

```c#
string account = "0xaca9b6d9b1636d99156bb12825c75de1e5a58870"; 
string authorizedOperator = "0x3482549fca7511267c9ef7089507c0f16ea1dcc1";

bool isApproved = await erc1155.IsApprovedForAll(account, authorizedOperator);
print(isApproved);
```
