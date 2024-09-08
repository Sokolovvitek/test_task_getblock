# test_task_getblock_`eth_getTransactionCount`
___
To work with the API, you need to create your own endpoint in your [account dashboard](https://account.getblock.io/):

|Protocol|Network|API/Add-on|
|:-:|:-:|:-:|
|**`ETHEREUM`**|**`Mainnet`** / **`Sepolia`**)|GraphQL / **`JSON-RPC`** / WebSocket|
___
1.	## **Description of the method `eth_getTransactionCount`:**

   *The **`eth_getTransactionCount`** method is used to retrieve the `nonce` value.* 

> *`Nonce` is a number that indicates the count of transactions sent from a specific Ethereum address. Its value is used to prevent the replay of transactions and to ensure the ordering of transactions.* 

*DApps can use this method to <u>determine how many transactions have been executed by a specific address</u>, which is useful for correctly forming new transactions.*

   **Example request:**
```json
{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params":["0xE73a62b0F9F93b3f992d8211d8E66E6a00D9bED1", "latest"],
    "id": "111ae1e11-11ab-11e1-1111-11b1f1f1e111"
}
```
###   **Description of request parameters**:
|Parameter|Data Type|Description|Example|
|:-:|:-:|:-:|:-:|
|"`Value`"|string|Indicates the blockchain state at the latest block.|`"earliest"`, `"pending"`, `"latest"` or a specific block number (e.g., `"0x10FB78"`). |
|"`0xAddress`"|string|Ethereum address in the format 0x...|0xE73a62b0F9F93b3f992d8211d8E66E6a00D9bED1|
|"`id`"|string|The id is an arbitrary value used to identify the request. It helps to link the request and response, especially when multiple requests are sent simultaneously.|24ae0e72-84ab-57e2-8819-09b4f6f9e911|
   

### Possible responses to the request:

 ***The response returns the number of transactions in hexadecimal format `hex` (0x…).***

#### Response with status «200 OK» 
```json 
"result": "0x0"
```
In JSON-RPC API Ethereum, a status of 200 OK means that the request was executed correctly and the result for the requested parameter is zero.

In the case of the **`eth_getTransactionCount`** method, this indicates that the specified Ethereum address has no transactions sent or the nonce is 0.

#### Response with status «200 OK». Error code 32602

```json
"message": "invalid argument 0: hex string has length 38, want 40 for common.Address" /// An error occurred when entering the parameter “0xAddress”; the number of characters after “0x” is incorrect (either less than or more than 40);
or
"message": "invalid argument 0: json: cannot unmarshal hex string of odd length into Go value of type common.Address" /// An odd number of characters was provided for the “0xAddress” parameter.
```
#### Response with status «200 OK». Error code 32700
```json
"message": "parse error"
```
Indicates a syntax error in the request – it is important to ensure that the parameters adhere to the specifications of the **`eth_getTransactionCount`** method. 

For example:
- The parameter `"earliest"`, `"latest"`, or `"pending"` must be in string format (enclosed in quotes) and free from typos/errors.
- The `id` must also be enclosed in quotes.

 #### Response with status «200 OK». Error code 32000
```json
"message": "missing trie node 00cd7bceda0982d3e861ad6f99046566052595232c762844f6bf3774724fd69a (path ) state 0x00cd7bceda0982d3e861ad6f99046566052595232c762844f6bf3774724fd69a is not available, not found"
```
The specified Ethereum node cannot be found in the state tree (`trie`) to process the request. Possible reasons include incomplete node synchronization, issues with local data storage, or the absence of required data on the archival node.

To resolve the issue, ensure that the node is fully synchronized, retry the request on a different node, or use an archival node with complete historical blockchain data.

To verify the correctness of the request, use parameter `"earliest"`, `"pending"`, or `"latest"` instead of a specific block number.
___
