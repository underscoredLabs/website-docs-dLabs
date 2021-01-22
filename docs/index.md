---
title: _dLabs SDK 
summary: A plugin bridging Ethereum Tokens to Unity.
---

# _dLabs Documentation

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

If [WalletScene](#walletscene) is not being used, then a private key is required.

```c#
string privateKey = "0000000000000000000000000000000000000000000000000000000000000001";
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

If [WalletScene](#walletscene) is not being used, then a private key is required.

```c#
string privateKey = "0000000000000000000000000000000000000000000000000000000000000001";
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

If [WalletScene](#walletscene) is not being used, then a private key is required.

```c#
string privateKey = "0000000000000000000000000000000000000000000000000000000000000001";
ERC1155 erc1155 = new ERC1155(network, contract, privateKey);

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

### Safe Transfer From

If [WalletScene](#walletscene) is not being used, then a private key is required.

```c#
string privateKey = "0000000000000000000000000000000000000000000000000000000000000001";
ERC721 erc721 = new ERC721(network, contract, privateKey);

string fromAccount = "0x72b8Df71072E38E8548F9565A322B04b9C752932";
string toAccount = "0xaCA9B6D9B1636D99156bB12825c75De1E5a58870";
string tokenId = "12";
string data = "0x0";

string transactionHash = await erc721.SafeTransferFrom(fromAccount, toAccount, tokenId, data);
print(transactionHash);
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

### Transfer

If [WalletScene](#walletscene) is not being used, then a private key is required.

```c#
string privateKey = "0000000000000000000000000000000000000000000000000000000000000001"; // remove if using WalletScene
ERC20 erc20 = new ERC20(network, contract, privateKey);

string toAccount = "0x72b8Df71072E38E8548F9565A322B04b9C752932";
string amount = "100000000000000000"; // in smallest unit

string transactionHash = await erc20.Transfer(toAccount, amount);
print(transactionHash);
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

Public and private key management can either be done through the WalletScene or manually.

### WalletScene

Add `/_dLabs/Scenes/WalletScene` to the beginning of your build settings. To access wallet scene private key and account:

```c#
string password = ""; // default is empty
string privateKey = UnityWallet.PrivateKey(password);
print(privateKey);

string account = UnityWallet.Account();
print(account);
```

### Manual

To generate your own account.

```c#
using System;
using NBitcoin;
using Nethereum.HdWallet;
using Nethereum.KeyStore.Model;

void GenerateWallet()
{
  // generate mneumonic
  Mnemonic mnemo = new Mnemonic(Wordlist.English, WordCount.Twelve);
  // create new wallet
  var wallet = new Wallet(mnemo.ToString(), "");
  // access Hierarchical Deterministic wallet 0
  var account = wallet.GetAccount(0);

  // encrypt account with password
  string password = "";
  var keyStoreService = new Nethereum.KeyStore.KeyStoreScryptService();
  var scryptParams = new ScryptParams { Dklen = 32, N = 262144, R = 1, P = 8 };
  var ecKey = new Nethereum.Signer.EthECKey(account.PrivateKey);
  var keyStore = keyStoreService.EncryptAndGenerateKeyStore(password, ecKey.GetPrivateKeyAsBytes(), ecKey.GetPublicAddress(), scryptParams);
  string json = keyStoreService.SerializeKeyStoreToJson(keyStore);

  // decrypt account
  print("account: " + account.Address);
  string decryptedPrivateKey = BitConverter.ToString(keyStoreService.DecryptKeyStoreFromJson(password, json)).Replace("-", string.Empty).Replace("0x", string.Empty).ToLower();
  print("decrypted private key: " + decryptedPrivateKey);
}
```

## OpenSea

OpenSea provides an HTTP API for fetching non-fungible assets based on a set of parameters.

### Definitions

Defines the structure of info and orders.

```c#
using OpenSeaDefinition;
```

### Get Token Info

```c#
string network = "mainnet"; // mainnet rinkeby
OpenSea openSea = new OpenSea(network);

string[] contracts = {"0x5e30b1d6f920364c847512e2528efdadf72a97a9"};
string[] tokenIds = {"9297472344724926092841919778025655237517835814992492894761274101626115719268", "9297472344724926092841919778025655237517835814992492894761274118118790135908"};
TokenInfo tokenInfo = await openSea.GetTokenInfo(contracts, tokenIds);

print(JsonUtility.ToJson(tokenInfo, true));
```

### Get Token Orders

```c#
string network = "rinkeby"; // mainnet rinkeby
OpenSea openSea = new OpenSea(network);

string[] contracts = { "0x2ebecabbbe8a8c629b99ab23ed154d74cd5d4342" };
string[] tokenIds = { "22" };
TokenOrders tokenOrders = await openSea.GetOrders(contracts, tokenIds);

print(JsonUtility.ToJson(tokenOrders, true));
```
