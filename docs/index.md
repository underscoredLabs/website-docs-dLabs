---
title: _dLabs SDK
summary: A plugin bridging Ethereum Tokens to Unity.
---

# \_dLabs Documentation

A plugin to bridge Unity to Ethereum Tokens.

## Introduction

[![Watch the video](https://user-images.githubusercontent.com/19412160/103559601-66aa0480-4e84-11eb-803a-6f854e93640a.png)](https://www.youtube.com/watch?v=_XnB1BjUSvQ)

## Ethereum

### Connect

```c#
string network = "mainnet"; // mainnet ropsten kovan rinkeby goerli
Ethereum ethereum = new Ethereum(network);
```

### Balance Of

```c#
string account = "0x99C85bb64564D9eF9A99621301f22C9993Cb89E3";

BigInteger wei = await ethereum.BalanceOf(account);
print(wei);
```

### Create Transaction

Create an unsigned transaction. User can sign offline.

```c#
string from = "0x35706484aB20Cbf22F5c7a375D5764DA8166aE1c";
string to = "0x7E3bE66431ba73956213C40C0828355D1A7894D3";
string eth = "1";

Transaction transaction = await ethereum.CreateTransaction(from, to, eth);

print("network: " + transaction.network);
print("to: " + transaction.to);
print("wei: " + transaction.wei);
print("nonce: " + transaction.nonce);
print("gasLimit: " + transaction.gasLimit);
print("gasPrice: " + transaction.gasPrice);
print("hex: " + transaction.hex);
```

### Broadcast Transaction

Broadcast a signed transaction.

```c#
string network = "rinkeby"; // mainnet ropsten kovan rinkeby goerli
Ethereum ethereum = new Ethereum(network);

// using the private key to sign transaction
string signedTransaction = "0xf86b04843b9aca0083989680947e3be66431ba73956213c40c0828355d1a7894d38703f18a03b36000802ca0043ab6289f2a44dd911bfb3658cfac12710354a3e0cef35544c9348b15f9f6f7a018d36b8d5b61dc00a54293528d0edd8a4a7c9f064817825c7e8cb8167b240860";

string transactionHash = await ethereum.Broadcast(signedTransaction);
print(transactionHash);
```

### Verify

Verify a signed message.

```c#
string network = "mainnet"; // mainnet ropsten kovan rinkeby goerli
Ethereum ethereum = new Ethereum(network);

string message = "YOUR_MESSAGE";
string signature = "0x94bdbebbd0180195b89721a55c3a436a194358c9b3c4eafd22484085563ff55e49a4552904266a5b56662b280757f6aad3b2ab91509daceef4e5b3016afd34781b";

string account = await ethereum.Verify(message, signature);
print (account); // 0xC52C1FB0B9681e1c80e5AdA8eEeD992C0C2706eE
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

## ERC721

### Connect

```c#
string network = "rinkeby"; // mainnet ropsten kovan rinkeby goerli
string contract = "0xe41d4b6ae91a322c89ecfe7670a154412659e8b0";
ERC721 erc721= new ERC721(network, contract);
```

### Balance Of

Counts all NFTs assigned to an owner

```c#
string account = "0x72b8Df71072E38E8548F9565A322B04b9C752932";
BigInteger balance = await erc721.BalanceOf(account);
print(balance);
```

### Owner Of

Find the owner of a NFT

```c#
string tokenId = "12";
string account = await erc721.OwnerOf(tokenId);
print(account);
```

## ERC20

### Connect

```c#
string network = "rinkeby"; // mainnet ropsten kovan rinkeby goerli
string contract = "0xc778417e063141139fce010982780140aa0cd5ab";
ERC20 erc20 = new ERC20(network, contract);
```

### Balance Of

```c#
string account = "0xaCA9B6D9B1636D99156bB12825c75De1E5a58870";
BigInteger balance = await erc20.BalanceOf(account);

// BigInteger decimals = await erc20.Decimals();
BigInteger decimals = 18;
print("blance in smallest unit: " + balance);
print("balance in decimals: " + (double)balance / (double)BigInteger.Pow(10, (int)decimals));
```

### Name

Returns the name of the token. E.g. "Wrapped Ether"

```c#
string name = await erc20.Name();
print(name)
```

### Symbol

Returns the symbol of the token. E.g. “WETH”.

```c#
string symbol = await erc20.Symbol();
print(symbol);
```

### Decimals

Returns the number of decimals the token uses - e.g. 8, means to divide the token amount by 100000000 to get its user representation.

```c#
BigInteger decimals = await erc20.Decimals();
print(decimals);
```

### Total Supply

Returns the total token supply.

```c#
BigInteger totalSupply = await erc20.TotalSupply();
print(totalSupply);
```

## Wallet

[![](https://user-images.githubusercontent.com/19412160/109343259-0d829f80-783b-11eb-9324-9962cd2b0c8c.png)](https://www.youtube.com/watch?v=PSydAY9ps-I)

### LoginScene

After Login Scene `_Config.cs` will store user info. This can be accessed in any scene.

```c#
string account = _Config.Account();
print(account);
```
