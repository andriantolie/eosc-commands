# EOSC

EOSC is a command line tool that can be used to interact with an EOS node (started by by the launcher EOSD). It is can be found inside the eos repository at https://github.com/EOSIO/eos/tree/master/programs/eosc. It is automatically built during the build process of EOSD.

Currently EOSC only supports communicating with EOSD when running on localhost port 8888. It communicates with EOSD through RPC over HTTP, therefore it's important that the node that is being communicated with has the chain_api_plugin enabled. 

To enable the chain_api_plugin on a node using EOSD, add the following line needs to be added to the data_dir/config.ini initialisation file of EOSD:
```
plugin = eos::chain_api_plugin
```

Below is the full list of commands available for command line tool (EOSC), according the status of August 24th 2017 EOS Master Branch, 
- [help](#help)
- [info](#info)
- [block](#block)
- [account](#account)
- [transfer](#transfer)
- [create](#create)
  - [create key](#create-key)
  - [create account](#create-account)
- [import](#import)
  - [import key](#import-key)
- [unlock](#unlock)
- [lock](#lock)
- [do](#do)
- [transaction](#transaction)
- [push-trx](#push-trx)
- [setcode](#setcode)
- [exec](#exec)

## help
- Description: show available command list
- Example:
    ```
    eosc help
    ```
    - Result:
      ```
      Command list: info, block, exec, account, push-trx, setcode, transfer, create, import, unlock, lock, and do
      ```
---
## info
- Description: show blockchain global properties
- Example:
    ```
    eosc info
    ```
    - Result:
      ```
      {
        "head_block_num": 21,
        "last_irreversible_block_num": 7,
        "head_block_id": "000000155e9d9af7774c8dc50ecb7fed3320811b660027e73a58ddeaeaa42096",
        "head_block_time": "2017-08-24T08:11:57",
        "head_block_producer": "inite",
        "recent_slots": "1111111111111111111111111111100000000000000000000000001111111111",
        "participation_rate": "0.60937500000000000"
      }
      ```
---
## block
- Description: get information about the specified block from the blockchain
- Arguments:
  ```
    eosc block <block_num_or_id>
  ```
  - block_num_or_id: block number or block id of the block to be queried
- Example:
    ```
    eosc block 21
    # or ./eosc block 000000155e9d9af7774c8dc50ecb7fed3320811b660027e73a58ddeaeaa42096
    ```
    - Result:
      ```
      {
        "previous": "0000001478768f8e6a35ba99b58385a3369c2c2392c98aada7a5ec6328c3bd82",
        "timestamp": "2017-08-24T08:11:57",
        "transaction_merkle_root": "0000000000000000000000000000000000000000000000000000000000000000",
        "producer": "inite",
        "producer_changes": [],
        "producer_signature": "2042989d38130ff1e6f0c9bf05085b709ddf5d8e83e7d5b32dc87a7eae35ffd5fc65ffebb34ebd20d4bcf3e9c1af91068759c38ded3c0a0ba92051401ed4646091",
        "cycles": [],
        "id": "000000155e9d9af7774c8dc50ecb7fed3320811b660027e73a58ddeaeaa42096",
        "block_num": 21,
        "refBlockPrefix": 3314371703
        }
      ```
---
## account
- Description: get account information
- Arguments:
  ```
    eosc account <account_name>
  ```
  - account_name: name of the account to be queried
- Example:
    ```
    eosc account inita
    ```
    - Result:
      ```
      {
        "name": "inita",
        "eos_balance": "100000000000000",
        "staked_balance": 0,
        "unstaking_balance": 0,
        "last_unstaking_time": "1969-12-31T23:59:59"
      }
      ```
---
## transfer
  - Description: transfer eos from one account to another account
  - Arguments:
    ```
      eosc transfer <sender_name> <recipient_name> <amount>
    ```
    - sender_name: account name of the sender
    - recipient_name: account name of the recipient
    - amount: amount of eos to be transferred
  - Example:
      ```
      eosc transfer eos tester 1000
      ```
      - Result:
        ```
        {
          "transaction_id": "52b488d27ce1f72a2b29f22e5e1638fa5db5d7805565884e795733a15c6c2195",
          "processed": {
            "refBlockNum": "25298",
            "refBlockPrefix": "1151709320",
            "expiration": "2017-07-25T17:58:58",
            "scope": [
              "eos",
              "tester"
            ],
            "signatures": [],
            "messages": [{
                "code": "eos",
                "type": "transfer",
                "authorization": [{
                    "account": "eos",
                    "permission": "active"
                  }
                ],
                "data": {
                  "from": "eos",
                  "to": "tester",
                  "amount": 1000
                },
                "hex_data": "e54d000000000000b44c5a2400000000e803000000000000"
              }
            ],
            "output": [{
                "notify": [{
                    "name": "tester",
                    "output": {
                      "notify": [],
                      "sync_transactions": [],
                      "async_transactions": []
                    }
                  }
                ],
                "sync_transactions": [],
                "async_transactions": []
              }
            ]
          }
        }
        ```
---
## create
  ### create key
  - Description: create a public and private key pair (which is necessary in order to create a new account later on)
  - Example:
      ```
      eosc create key
      ```
      - Result:
        ```
        public: EOS5YuxWBox8moiBuFJAzQdiXLjQfXMyfaYwN4BaN1coSJM6mYCpZ
        private: 5KPK5niqgE5pXsKkYn8zgtRnL5BFmPMp4nv8sNSdTMyZQiuhxzE
        ```
  ### create account
  - **SIGNING TRANSACTION IS NOT SUPPORTED YET**
  - Description: create a new account
  - Arguments:
    ```
      eosc create account <creator> <new_account_name> <owner_public_key> <active_public_key>
    ```
    - creator: account name of the account that creates the new account (i.e. the one who pays for the account creation)
    - new_account_name: name of the account to be created
    - owner_public_key: owner public key of the account to be created
    - active_public_key: active public key of the account to be created
  - Example:
      ```
      eosc create account inita tester EOS4toFS3YXEQCkuuw1aqDLrtHim86Gz9u3hBdcBw5KNPZcursVHq EOS6KdkmwhPyc2wxN9SAFwo2PU2h74nWs7urN1uRduAwkcns2uXsa
      ```
      - Result:
          ```
          {
            "transaction_id": "6acd2ece68c4b86c1fa209c3989235063384020781f2c67bbb80bc8d540ca120",
            "processed": {
              "refBlockNum": "25217",
              "refBlockPrefix": "2095475630",
              "expiration": "2017-07-25T17:54:55",
              "scope": [
                "eos",
                "inita"
              ],
              "signatures": [],
              "messages": [{
                  "code": "eos",
                  "type": "newaccount",
                  "authorization": [{
                      "account": "inita",
                      "permission": "active"
                    }
                  ],
                  "data": "c9251a0000000000b44c5a2400000000010000000102bcca6347d828d4e1868b7dfa91692a16d5b20d0ee3d16a7ca2ddcc7f6dd03344010000010000000102bcca6347d828d4e1868b7dfa91692a16d5b20d0ee3d16a7ca2ddcc7f6dd03344010000010000000001c9251a000000000061d0640b000000000100010000000000000008454f5300000000"
                }
              ],
              "output": [{
                  "notify": [],
                  "sync_transactions": [],
                  "async_transactions": []
                }
              ]
            }
          }
          ```
---
## import
  ### import key
  - Description: generate public key from given private key
  - Arguments:
    ```
      eosc import key <private_key>
    ```
    - private_key: private key of the public key to be generated
  - Example:
      ```
      eosc import key 5KPK5niqgE5pXsKkYn8zgtRnL5BFmPMp4nv8sNSdTMyZQiuhxzE
      ```
      - Result:
          ```
          public: EOS5YuxWBox8moiBuFJAzQdiXLjQfXMyfaYwN4BaN1coSJM6mYCpZ
          ```
---
## unlock
  - **NOT IMPLEMENTED YET**
---
## lock
  - **NOT IMPLEMENTED YET**
---
## do
  - **NOT IMPLEMENTED YET**
---
## transaction
- Description: get information of the specified transaction ID from the blockchain
- Arguments:
  ```
    eosc transaction <transaction_id>
  ```
  - transaction_id: transaction id of the block to be queried
- Example:
    ```
    eosc transaction 52b488d27ce1f72a2b29f22e5e1638fa5db5d7805565884e795733a15c6c2195
    ```
    - Result:
      ```
      {
        "transaction_id": "52b488d27ce1f72a2b29f22e5e1638fa5db5d7805565884e795733a15c6c2195",
        "processed": {
          "refBlockNum": "25298",
          "refBlockPrefix": "1151709320",
          "expiration": "2017-07-25T17:58:58",
          "scope": [
            "eos",
            "tester"
          ],
          "signatures": [],
          "messages": [{
              "code": "eos",
              "type": "transfer",
              "authorization": [{
                  "account": "eos",
                  "permission": "active"
                }
              ],
              "data": {
                "from": "eos",
                "to": "tester",
                "amount": 1000
              },
              "hex_data": "e54d000000000000b44c5a2400000000e803000000000000"
            }
          ],
          "output": [{
              "notify": [{
                  "name": "tester",
                  "output": {
                    "notify": [],
                    "sync_transactions": [],
                    "async_transactions": []
                  }
                }
              ],
              "sync_transactions": [],
              "async_transactions": []
            }
          ]
        }
      }
      ```
---
## push-trx
- **SIGNING TRANSACTION IS NOT SUPPORTED YET**
- Description: push a transaction
- Arguments:
  ```
    eosc push-trx <transaction_json_string>
  ```
  - transaction_json_string: transaction in json string format
- Example:
    ```
      eosc push-trx "{"refBlockNum":"25298","refBlockPrefix":"1151709320","expiration":"2017-07-25T17:58:58","scope":["eos","tester"],"messages":[{"code":"eos","type":"transfer","authorization":[{"account":"eos","permission":"active"}],"data":"000000000000e62b00000000c84267a1e803000000000000"}],"signatures":[],"authorizations":[]}"
    ```
    - Result:
        ```
        {
          "transaction_id": "52b488d27ce1f72a2b29f22e5e1638fa5db5d7805565884e795733a15c6c2195",
          "processed": {
            "refBlockNum": "25298",
            "refBlockPrefix": "1151709320",
            "expiration": "2017-07-25T17:58:58",
            "scope": [
              "eos",
              "tester"
            ],
            "signatures": [],
            "messages": [{
                "code": "eos",
                "type": "transfer",
                "authorization": [{
                    "account": "eos",
                    "permission": "active"
                  }
                ],
                "data": {
                  "from": "eos",
                  "to": "tester",
                  "amount": 1000
                },
                "hex_data": "e54d000000000000b44c5a2400000000e803000000000000"
              }
            ],
            "output": [{
                "notify": [{
                    "name": "tester",
                    "output": {
                      "notify": [],
                      "sync_transactions": [],
                      "async_transactions": []
                    }
                  }
                ],
                "sync_transactions": [],
                "async_transactions": []
              }
            ]
          }
        }
        ```
---
## setcode
- **SIGNING TRANSACTION IS NOT SUPPORTED YET**
- Description: create and publish a contract (note that the account in the contract_name field needs to be created first)
- Arguments:
  ```
    eosc setcode <contract_name> <contract_wast_path> <contract_abi_path>
  ```
  - contract_name: name of the contract
  - contract_wast_path: path to contract wast file
  - contract_abi_path: path to contract abi file
- Example:
    ```
      eosc setcode currency ../../contracts/currency/currency.wast ../../contracts/currency/currency.abi
    ```
    - Result:
        ```
        {
          "transaction_id": "738669518a8fc6935394992beec1dc4dc1b60f7d9f232b6ccd6a282619eedca9",
          "processed": {
            "refBlockNum": "25386",
            "refBlockPrefix": "154808726",
            "expiration": "2017-07-25T18:03:22",
            "scope": [
              "eos",
              "currency"
            ],
            "signatures": [],
            "messages": [{
                "code": "eos",
                "type": "setcode",
                "authorization": [{
                    "account": "currency",
                    "permission": "active"
                  }
                ],
                "data": "a34a59dcc80000000000f7090061736d0100000001390a60037e7e7e017f60047e7e7f7f017f60017e0060057e7e7e7f7f017f60027f7f0060027f7f017f60027e7f0060017f0060000060027e7e0002760703656e7606617373657274000403656e76086c6f61645f693634000303656e760b726561644d657373616765000503656e760a72656d6f76655f693634000003656e760b7265717569726541757468000203656e760d726571756972654e6f74696365000203656e760973746f72655f6936340001030504060708090404017000000503010001077e05066d656d6f727902002a5f5a4e3863757272656e6379313273746f72654163636f756e744579524b4e535f374163636f756e74450007355f5a4e3863757272656e637932336170706c795f63757272656e63795f7472616e7366657245524b4e535f385472616e7366657245000804696e69740009056170706c79000a0aaa04043100024020012903084200510d00200042e198deead1002001411010061a0f0b200042e198deead100200129030010031a0bd30202017e017f4100410028020441206b220236020420002903002101200029030810052001100520002903001004200242e198deead10037031020024200370318200029030042a395e5e28d1942e198deead100200241106a411010011a200242e198deead10037030020024200370308200029030842a395e5e28d1942e198deead1002002411010011a200229031820002903105a4110100020022002290318200029031022017d370318200120022903087c20015a41c00010002002200229030820002903107c370308200029030021010240024020022903184200510d00200142e198deead100200241106a411010061a0c010b200142e198deead100200229031010031a0b200041086a290300210102400240200241086a2903004200510d00200142e198deead1002002411010061a0c010b200142e198deead100200229030010031a0b4100200241206a3602040b4901017f4100410028020441106b22003602042000428094ebdc03370308200042e198deead10037030042a395e5e28d1942e198deead1002000411010061a4100200041106a3602040b5701017f4100410028020441206b22023602040240200042a395e5e28d19520d00200142d48cdce99412520d0020024200370318200241086a4118100241174b41f0001000200241086a10080b4100200241206a3602040b0b8b01040041040b04900400000041100b2c696e746567657220756e646572666c6f77207375627472616374696e6720746f6b656e2062616c616e6365000041c0000b26696e7465676572206f766572666c6f7720616464696e6720746f6b656e2062616c616e6365000041f0000b1e6d6573736167652073686f72746572207468616e2065787065637465640000ec01046e616d650b06617373657274020000086c6f61645f6936340500000000000b726561644d6573736167650200000a72656d6f76655f693634030000000b726571756972654175746801000d726571756972654e6f7469636501000973746f72655f69363404000000002a5f5a4e3863757272656e6379313273746f72654163636f756e744579524b4e535f374163636f756e74450201300131355f5a4e3863757272656e637932336170706c795f63757272656e63795f7472616e7366657245524b4e535f385472616e73666572450301300131013204696e6974010130056170706c7903013001310132010b4163636f756e744e616d65044e616d6502087472616e7366657200030466726f6d0b4163636f756e744e616d6502746f0b4163636f756e744e616d6506616d6f756e740655496e743634076163636f756e740002036b65790655496e7436340762616c616e63650655496e743634015406374d91000000087472616e7366657201618c571d05000000076163636f756e74"
              }
            ],
            "output": [{
                "notify": [],
                "sync_transactions": [],
                "async_transactions": []
              }
            ]
          }
        }
        ```
---
## exec
- **SIGNING TRANSACTION IS NOT SUPPORTED YET**
- Description: execute a contract's action
- Arguments:
  ```
    eosc exec <contract_name> <action_name> <action_arguments> <scope> <permission>
  ```
  - contract_name: name of the contract
  - action_name: name of the contract's action to be executed
  - action_arguments: arguments for the contract's action
  - scope: array of account names that are involved in the contract's action
  - permission: the accounts and permission levels provided
- Example:
    ```
      eosc exec currency transfer '{"from":"currency","to":"tester","amount":50}' '["currency","tester"]' '[{"account":"currency","permission":"active"}]'
    ```
    - Result:
        ```
        {
          "transaction_id": "f601f2fdb26e366a19913229e4d2928778b50166811c63c7962401b11d23ef3d",
          "processed": {
            "refBlockNum": "25427",
            "refBlockPrefix": "2231248056",
            "expiration": "2017-07-25T18:05:25",
            "scope": [
              "tester",
              "currency"
            ],
            "signatures": [],
            "messages": [{
                "code": "currency",
                "type": "transfer",
                "authorization": [{
                    "account": "currency",
                    "permission": "active"
                  }
                ],
                "data": {
                  "from": "currency",
                  "to": "tester",
                  "amount": 50
                },
                "hex_data": "a34a59dcc8000000b44c5a24000000003200000000000000"
              }
            ],
            "output": [{
                "notify": [{
                    "name": "tester",
                    "output": {
                      "notify": [],
                      "sync_transactions": [],
                      "async_transactions": []
                    }
                  }
                ],
                "sync_transactions": [],
                "async_transactions": []
              }
            ]
          }
        }
        ```
