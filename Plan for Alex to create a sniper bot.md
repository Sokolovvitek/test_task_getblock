Plan for Alex to create a sniper bot

___

To work with the API, you need to create your own endpoint in your [account dashboard](https://account.getblock.io/):

|Protocol|Network|API/Add-on|
|:-:|:-:|:-:|
|**`SOLANA`**|**`Mainnet`**|**`JSON-RPC`** / WebSocket|

___

### Step 1: Get the Latest Block Height
Objective: Determine the current block height on Solana.

#### Action:

Send a request to **`getblockheight`** to get the current block height.

```json
{
    "jsonrpc": "2.0",
    "method": "getBlockHeight",
    "params": [null],
    "id": "getblock.io"
}
```

##### Example Response:

```json
{
    "jsonrpc": "2.0",
    "result": 267695004,
    "id": "getblock.io"
}
```

### Step 2: Get Information about the Latest Block
Objective: Retrieve data about the latest block, including the list of transactions.

#### Action:

Send a request to **`getblock`** with the height of the latest block.

```json
{
    "jsonrpc": "2.0",
    "method": "getBlock",
    "params": [
        288414313,
        {
            "maxSupportedTransactionVersion": 0
        }
    ],
    "id": "getblock.io"
}
```

##### Example Response:

```json
{
    "jsonrpc": "2.0",
    "result": {
        "blockHeight": 267377237,
        "blockTime": 1725738547,
        "blockhash": "DL1BhC5iJyYN9eyY7vV54pYAoYQxDzMadW4hjx6XAUSC",
        "parentSlot": 288414312,
        "previousBlockhash": "3sJBuijtfQ9y1UQfeARNaiJDrExBLSEphytoqTUirTZa",
        "rewards": [
            {
                "commission": null,
                "lamports": 5999837,
                "postBalance": 7152909316729,
                "pubkey": "EkvdKhULbMFqjKBKotAzGi3kwMvMpYNDKJXXQQmi6C1f",
                "rewardType": "Fee"
            }
        ],
        "transactions": [
            {
                "meta": {
                    "computeUnitsConsumed": 6654,
                    "err": {
                        "InstructionError": [
                            1,
                            {
                                "Custom": 6003
                            }
                        ]
                    },
                    "fee": 5400,
                    "innerInstructions": [],
                    "loadedAddresses": {
                        "readonly": [],
                        "writable": []
                    },
                    "logMessages": [
                        "Program ComputeBudget111111111111111111111111111111 invoke [1]",
                        "Program ComputeBudget111111111111111111111111111111 success",
                        "Program cjg3oHmg9uuPsP8D6g29NWvhySJkdYdAo9D25PRbKXJ invoke [1]",
                        "Program log: AnchorError thrown in programs/ocr2/src/lib.rs:639. Error Code: StaleReport. Error Number: 6003. Error Message: Stale report.",
                        "Program cjg3oHmg9uuPsP8D6g29NWvhySJkdYdAo9D25PRbKXJ consumed 6504 of 199850 compute units",
                        "Program cjg3oHmg9uuPsP8D6g29NWvhySJkdYdAo9D25PRbKXJ failed: custom program error: 0x1773"
                    ],
                    "postBalances": [
                        22169752529,
                        49054080,
                        2616960,
                        1141440,
                        0,
                        0,
                        1141440,
                        1
                    ],
                    "postTokenBalances": [],
                    "preBalances": [
                        22169757929,
                        49054080,
                        2616960,
                        1141440,
                        0,
                        0,
                        1141440,
                        1
                    ],
                    "preTokenBalances": [],
                    "rewards": [],
                    "status": {
                        "Err": {
                            "InstructionError": [
                                1,
                                {
                                    "Custom": 6003
                                }
                            ]
                        }
                    }
                },
                "transaction": {
and so on
```

### Step 3: Get Information about Transactions
Objective: Obtain details about the transactions mentioned in the block.

#### Action:

Send a request to **`getconfirmedtransaction`** for each transaction from the previous step.

```json
{
    "jsonrpc": "2.0",
    "method": "getConfirmedTransaction",
    "params": [
        null, 
        null
    ],
    "id": "getblock.io"
}
```
If you need to pass specific values ​​to `params`, replace `null` with the appropriate data.

##### Example Response:

```json
{
    "jsonrpc": "2.0",
    "result": {
        "transaction": {
            "signatures": ["TRANSACTION_SIGNATURE"],
            "message": {
                "accountKeys": ["Account1", "Account2"],
                "instructions": [
                    {
                        "programIdIndex": 1,
                        "accounts": [0, 1],
                        "data": "..."
                    }
                ]
            }
        },
        "meta": {
            "fee": 5000,
            "preBalances": [1000000, 2000000],
            "postBalances": [995000, 1995000]
        }
    },
    "id": "getblock.io"
}
```

### Step 4: Get Program Account Information
Objective: Identify which programs and contracts are in the block.

#### Action:

Send a request to **`getprogramaccounts`** with the program address if needed.

```json
{
    "jsonrpc": "2.0",
    "method": "getProgramAccounts",
    "params": [
        "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
        null
    ],
    "id": "getblock.io"
}
```
If the request is successful, you will receive information about all accounts that are associated with the specified address.

##### Example Response:

```json
{
    "jsonrpc": "2.0",
    "result": [
        {
            "pubkey": "Account1",
            "account": {
                "data": [
                    "Base64_encoded_data"
                ],
                "executable": false,
                "lamports": 1000000,
                "owner": "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
                "rentEpoch": 123
            }
        },
        {
            "pubkey": "Account2",
            "account": {
                "data": [
                    "Base64_encoded_data"
                ],
                "executable": true,
                "lamports": 2000000,
                "owner": "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
                "rentEpoch": 123
            }
        }
    ],
    "id": "getblock.io"
}
```

### Step 5: Get List of Confirmed Transactions for an Address
Objective: Find all transactions related to a specific address.

#### Action:

Send a request to **`getconfirmedsignaturesforaddress2`** for the address you want to monitor.

```json
{
    "jsonrpc": "2.0",
    "method": "getConfirmedSignaturesForAddress2",
    "params": ["BGet7hFNtmGiZSRdhdnqXDJeXTN9AVBuLNCV9knsFawv",{"limit": 10}],
    "id": "getblock.io"
}
```
The **`getConfirmedSignaturesForAddress2`** method requires at least one valid parameter, usually an address.

##### Example Response:


```json
{
    "jsonrpc": "2.0",
    "result": [
        {
            "blockTime": 1725875870,
            "confirmationStatus": "finalized",
            "err": null,
            "memo": null,
            "signature": "3pcFjrTMZ5ARFLqfyAYMYV1xhUyobrVCt5k2j6boJvmaSraNzaKmdYsu6syujdkkgy3zWE29T8e4SHi3S7zQfidY",
            "slot": 288741600
        },
        {
            "blockTime": 1725875869,
            "confirmationStatus": "finalized",
            "err": {
                "InstructionError": [
                    4,
                    {
                        "Custom": 6001
                    }
                ]
            },
            "memo": null,
            "signature": "29GkNf27PHN3atXKG8PXmrq74LzexDWn4VaQN9EgZyBi7Hv79rLe9Mfn6qqsqRxXzQ7fQwWZzzQ3BjkcQytjD99v",
            "slot": 288741599
        },
        {
            "blockTime": 1725875862,
            "confirmationStatus": "finalized",
            "err": null,
            "memo": null,
            "signature": "4F2oyvderNgHvtuUpHYaou5AtMRFQ8aLXnuZCPb5pJnMpnZYeNBzUEgJ8zsvP7ZhuNhJtUH8p3wwgdxD7B7DmFF4",
            "slot": 288741583
        },
        {
            "blockTime": 1725875847,
            "confirmationStatus": "finalized",
            "err": null,
            "memo": null,
            "signature": "2S4pKMVNLsHr95mg8gSD7zT6peDeGDQWcTLVyxySyoohCWqMdf6qL75XQoL4tWeYEkUJ13wcKwhv9xZAsY5oZ2DK",
            "slot": 288741547
        },
        {
            "blockTime": 1725875843,
            "confirmationStatus": "finalized",
            "err": null,
            "memo": null,
            "signature": "44MifnXofHfTPEiSpzXKio3DqfsT7rDsfBZPU5RMapAQj4G9s39jZoQbVohxqq2Zukyb8rxUJR92AjTtZkxZ8Ng1",
            "slot": 288741536
        },
        {
            "blockTime": 1725875837,
            "confirmationStatus": "finalized",
            "err": null,
            "memo": null,
            "signature": "2MVV1uahmxjTB7eAvDyZ41hGNaE1eLqK33BJcRRdwb8G8A83kT9LaVRTRLEnDbvQttegZvGD6GoeeH8r3UeUkmW3",
            "slot": 288741522
        },
        {
            "blockTime": 1725875835,
            "confirmationStatus": "finalized",
            "err": null,
            "memo": null,
            "signature": "3Q5jLcoTbN5iji6Y4ymqV8CUaBUVbUBPZ7pAa1iXXUG4z2EjiPk5t6EXJmxmKmQEANwKxnRNE2Hqh4WGDTz5Hv8V",
            "slot": 288741519
        },
        {
            "blockTime": 1725875835,
            "confirmationStatus": "finalized",
            "err": null,
            "memo": null,
            "signature": "4N1NbB5nQwSD7UsZbQj7Bi5ErWv3Kjw2r2VsdFq9eK78wS64RpFqnCr5qe3xhtiTDvkwnrVzyJbPsfbFY4mJ9mNB",
            "slot": 288741518
        },
        {
            "blockTime": 1725875821,
            "confirmationStatus": "finalized",
            "err": null,
            "memo": null,
            "signature": "5Vq7GN2HxKVanQwvirxsTd9sDERCM53VLijt4hSffVcjYY48NUNvepfKi8kEqALVEMCdnAPcsWkvZNANCeK7YQok",
            "slot": 288741485
        },
        {
            "blockTime": 1725875820,
            "confirmationStatus": "finalized",
            "err": null,
            "memo": null,
            "signature": "2xpLYTVZXrtv6zgydhXefedu9yoefCR8yM9A25m5EmktRRNzkJ3CJKy5V5uNGkjaNbnX66QdgH576ii6anV4zZUJ",
            "slot": 288741482
        }
    ],
    "id": "getblock.io"
}
```

These steps will help Alex obtain current information about new meme coins on Solana by determining the latest block, retrieving transaction details, and analyzing program accounts and transaction histories.
___
___
___
___
___

### Alex will most likely need to use the Python development language.

#### 1: Introduction to Solana Blockchain and the GetBlock API
Goal: Understand the basics of the Solana blockchain and how to interact with it through the GetBlock API.

Solana Review: A brief introduction to Solana and its key features.
GetBlock API: Overview of available JSON-RPC and WebSocket endpoints for Solana.

```python
import requests

# Example of getting the height of the last block
def get_block_height():
    url = "https://go.getblock.io/a7b6cfa967dc42b8879eb4b74942d0c1/getblockheight"
    response = requests.get(url)
    return response.json()

print(get_block_height())
```

#### 2: Setting Up Your Environment
Goal: Set up a working environment, including installing the necessary libraries and tools.

Setting up the Python environment: Installing the necessary libraries (requests, websockets).
API Keys: How to securely store and use API keys.

```bash
pip install requests websockets
```

#### 3: Accessing and Analyzing Solana Transactions
Objective: Learn how to use the GetBlock API to retrieve and analyze Solana transactions.

Retrieving Transactions: Use **`getconfirmedtransaction`** to retrieve transaction details.
Transaction Filtering: Extracting relevant transaction data to find new memcoin deployments.

```python
def get_transaction(transaction_signature):
    url = f"https://go.getblock.io/a7b6cfa967dc42b8879eb4b74942d0cb/getconfirmedtransaction/{transaction_signature}"
    response = requests.get(url)
    return response.json()

# Usage example
signature = "5c6d8b6bba4d8e7d4e4568f5bb6a5a9c95b6e8d0e1d7e8a1"
print(get_transaction(signature))
```

#### 4: Using WebSocket to Receive Real-Time Updates
Goal: Implement WebSocket to receive real-time updates on new transactions.

WebSocket Connection: Use wss://go.getblock.io/<YOUR-ACCESS-TOKEN>/mainnet to receive real-time data.
Real-time data processing: Processing incoming messages to detect new memcoin contracts.

```python
import asyncio
import websockets

async def listen():
    uri = "wss://go.getblock.io/54b45410752049009a205bdaace79394"
    async with websockets.connect(uri) as websocket:
        while True:
            message = await websocket.recv()
            print(message)

# Start the WebSocket listener
asyncio.get_event_loop().run_until_complete(listen())
```

#### 5: Identifying New Memcoins
Goal: Write logic to identify and track new memcoin contracts.

Contract identification: Use transaction data to discover contracts related to memcoins.
Data Storage: Storing identified memcoins in a database or file for further analysis.

```python
def is_meme_coin_transaction(transaction_data):
    # Logic to determine if a transaction is related to memcoin
    # This can be based on transaction metadata or contract addresses
    return "meme_coin" in transaction_data

def process_transactions(transaction_list):
    for transaction in transaction_list:
        data = get_transaction(transaction)
        if is_meme_coin_transaction(data):
            print(f"New memecoin discovered: {data}")
```

#### 6: Completion and Testing
Goal: Complete the development of the tool and test it to ensure correct operation.

Testing: Run tests to ensure the bot accurately detects new memcoins.
Deployment: Tips for deploying the tool, including possible performance optimizations.

```python
def main():
    transactions = fetch_recent_transactions()
    process_transactions(transactions)

if __name__ == "__main__":
    main()
```
This guide will provide Alex with all the necessary steps to create and deploy a tool to track new memcoins on Solana using the GetBlock API.
___
___
