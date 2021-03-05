---
title: _dLabs SDK
summary: A plugin bridging Ethereum Tokens to Unity.
---

# \_dLabs Documentation

A plugin to bridge Unity to Ethereum Tokens.

## Introduction

[![Watch the video](https://user-images.githubusercontent.com/19412160/103559601-66aa0480-4e84-11eb-803a-6f854e93640a.png)](https://www.youtube.com/watch?v=_XnB1BjUSvQ)

## Ethereum

### Balance Of

```c#
string network = "mainnet"; // mainnet ropsten kovan rinkeby goerli
string account = "0x99C85bb64564D9eF9A99621301f22C9993Cb89E3";

BigInteger wei = await Ethereum.BalanceOf(network, account);
print("wei: " + wei);
```

### Create Transaction

Create an unsigned transaction. User can sign offline.

```c#
string network = "rinkeby"; // mainnet ropsten kovan rinkeby goerli
string from = "0x35706484aB20Cbf22F5c7a375D5764DA8166aE1c";
string to = "0x7E3bE66431ba73956213C40C0828355D1A7894D3";
string eth = "0.00111";

Transaction transaction = await Ethereum.CreateTransaction(network, from, to, eth);

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
string signedTransaction = "0xf86b04843b9aca0083989680947e3be66431ba73956213c40c0828355d1a7894d38703f18a03b36000802ca0043ab6289f2a44dd911bfb3658cfac12710354a3e0cef35544c9348b15f9f6f7a018d36b8d5b61dc00a54293528d0edd8a4a7c9f064817825c7e8cb8167b240860";

string transactionHash = await Ethereum.Broadcast(network, signedTransaction);
print (transactionHash);
```

### Verify

Verify a signed message.

```c#
string network = "mainnet"; // mainnet ropsten kovan rinkeby goerli
string message = "YOUR_MESSAGE";
string signature = "0x94bdbebbd0180195b89721a55c3a436a194358c9b3c4eafd22484085563ff55e49a4552904266a5b56662b280757f6aad3b2ab91509daceef4e5b3016afd34781b";

string account = await Ethereum.Verify(network, message, signature);
print (account); // 0xC52C1FB0B9681e1c80e5AdA8eEeD992C0C2706eE
```

## ERC1155

ERC-1155 contracts have two additonal fields.

- Contract address: The address that stores the balances of the token.

- Token Id: The specific token inside of the contract address.

For example. The contract address `0x5e30b1d6f920364c847512e2528efdadf72a97a9` can store all digital chess pieces. A pawn can be token id `17`.

### Balance Of

```c#
string network = "rinkeby"; // mainnet ropsten kovan rinkeby goerli
string contract = "0x2ebecabbbe8a8c629b99ab23ed154d74cd5d4342";
string account = "0xaca9b6d9b1636d99156bb12825c75de1e5a58870";
string tokenId = "22";

BigInteger balance = await ERC1155.BalanceOf(network, contract, account, tokenId);

print (balance)
```

### Balance Of Batch

Balance of batch will get the balance of a list of accounts and token ids. For example:

Get the balance of account `0xaCA9B6D9B1636D99156bB12825c75De1E5a58870` with token id `17` and

balance of account `0xaCA9B6D9B1636D99156bB12825c75De1E5a58870` with token id `22`

```c#
string network = "rinkeby"; // mainnet ropsten kovan rinkeby goerli
string contract = "0x2ebecabbbe8a8c629b99ab23ed154d74cd5d4342";
string[] accounts ={ "0xaCA9B6D9B1636D99156bB12825c75De1E5a58870", "0xaCA9B6D9B1636D99156bB12825c75De1E5a58870" };
string[] tokenIds = { "17", "22" };

List<BigInteger> batchBalances = await ERC1155.BalanceOfBatch(network, contract, accounts, tokenIds);

foreach (var balance in batchBalances)
{
  print ("batchBalance:" + balance);
}
```

### URI

Returns meta data about the token.

```c#
string network = "rinkeby"; // mainnet ropsten kovan rinkeby goerli
string contract = "0x2ebecabbbe8a8c629b99ab23ed154d74cd5d4342";
string tokenId = "17";

string uri = await ERC1155.URI(network, contract, tokenId);

print (uri);
```

### Is Approved For All

Queries the approval status of an operator for a given owner

```c#
string network = "rinkeby"; // mainnet ropsten kovan rinkeby goerli
string contract = "0x2ebecabbbe8a8c629b99ab23ed154d74cd5d4342";
string account = "0xaca9b6d9b1636d99156bb12825c75de1e5a58870";
string authorizedOperator = "0x3482549fca7511267c9ef7089507c0f16ea1dcc1";

// Queries the approval status of an operator for a given owner
bool isApproved = await ERC1155.IsApprovedForAll(network, contract, account, authorizedOperator);

print (isApproved);
```

## ERC721

### Balance Of

Counts all NFTs assigned to an owner

```c#
string network = "mainnet"; // mainnet ropsten kovan rinkeby goerli
string contract = "0x60f80121c31a0d46b5279700f9df786054aa5ee5";
string account = "0x6b2be2106a7e883f282e2ea8e203f516ec5b77f7";

BigInteger balance = await ERC721.BalanceOf(network, contract, account);

print (balance);
```

### Owner Of

Find the owner of a NFT

```c#
string network = "mainnet"; // mainnet ropsten kovan rinkeby goerli
string contract = "0xf5b0a3efb8e8e4c201e2a935f110eaaf3ffecb8d";
string tokenId = "721";

string account = await ERC721.OwnerOf(network, contract, tokenId);

print (account);
```

## ERC20

### Balance Of

```c#
string network = "rinkeby"; // mainnet ropsten kovan rinkeby goerli
string contract = "0xc778417e063141139fce010982780140aa0cd5ab";
string account = "0xaCA9B6D9B1636D99156bB12825c75De1E5a58870";

BigInteger balance = await ERC20.BalanceOf(network, contract, account);

print (balance);
```

### Name

Returns the name of the token. E.g. "Wrapped Ether"

```c#
string network = "rinkeby"; // mainnet ropsten kovan rinkeby goerli
string contract = "0xc778417E063141139Fce010982780140Aa0cD5Ab";

string name = await ERC20.Name(network, contract);

print (name);
```

### Symbol

Returns the symbol of the token. E.g. “WETH”.

```c#
string network = "rinkeby"; // mainnet ropsten kovan rinkeby goerli
string contract = "0xc778417E063141139Fce010982780140Aa0cD5Ab";

string symbol = await ERC20.Symbol(network, contract);

print (symbol);
```

### Decimals

Returns the number of decimals the token uses - e.g. 8, means to divide the token amount by 100000000 to get its user representation.

```c#
string network = "rinkeby"; // mainnet ropsten kovan rinkeby goerli
string contract = "0xc778417E063141139Fce010982780140Aa0cD5Ab";

BigInteger decimals = await ERC20.Decimals(network, contract);

print (decimals);
```

### Total Supply

Returns the total token supply.

```c#
string network = "rinkeby"; // mainnet ropsten kovan rinkeby goerli
string contract = "0xc778417e063141139fce010982780140aa0cd5ab";

BigInteger totalSupply = await ERC20.TotalSupply(network, contract);

print (totalSupply);
```

## Wallet

[![](https://user-images.githubusercontent.com/19412160/109343259-0d829f80-783b-11eb-9324-9962cd2b0c8c.png)](https://www.youtube.com/watch?v=PSydAY9ps-I)

### LoginScene

After Login Scene `_Config.cs` will store user info. This can be accessed in any scene.

```c#
string account = _Config.Account;
print(account);
```
