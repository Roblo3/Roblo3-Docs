# DynamoDB

## Service Resource
An object representing DynamoDB as a high-level resource:

```lua
local roblo3 = require(game:GetService("ServerScriptService").Roblo3)

local dynamodb = roblo3.resource("dynamodb")
```

### `dynamodb:Table`
Returns an object representing the DynamoDB table with the given name:

```lua
local table = dynamodb.Table("name")
```

??? Info "Parameters"
    | Parameter Name | Type   | Required | Description                    |
    | -------------- | ------ | -------- | ------------------------------ |
    | `name`         | String | Yes      | The name of the DynamoDB table |

??? Success "Return Value"
    | Return Name                        | Description                                      |
    | ---------------------------------- | ------------------------------------------------ |
    | [`dynamodb.Table`](#dynamodbtable) | An object representing the DynamoDB table given |

???+ Info "Functions"
    | Function Name  | Description                        |
    | -------------- | ---------------------------------- |
    | [`GetItem`](#getitem) | Gets the given item from DynamoDB table |
    | [`GetTableInfo`](#gettableinfo) | Gets info about the table from AWS |

#### `GetItem`
Returns the requested item, based on the key provided.

```lua
local item = table:GetItem(
    ["Key"] = {
        ["key name"] = "key value"
    }
)
```

??? Info "Parameters"
    `GetItem` does not take parameters in normal Lua fashion. Instead, it takes a dictionary of key-value pairs and parses them to its arguments. The values shown below are parsed:

    | Parameter Name | Type       | Required | Description |
    | -------------- | ---------- | -------- | ----------- |
    | Key            | Dictionary | Yes      | The key-value pair corresponding to the key provided when you created the DynamoDB table. If you specified a simple key, only the primary key should be provided. If you specified a composite key, both the primary key and sort key must be provided. |

??? Success "Return Value"
    | Return Name | Description |
    | ----------- | ----------- |
    | Item        | A Lua table with the requested item, or `nil` if DynamoDB didn't return an item (i.e., there wasn't an item that matched the key provided). The structure of this item will be dependent on your DynamoDB table structure and the item DynamoDB returned.
    | Response Data | A string of JSON data that is the raw, unparsed response from DynamoDB.
    | Raw Response  | A Lua table of the raw response, semi-parsed response from the `Requests` handler. This contains extra data that the `Requests` handler uses to determine automatic-retry and error propagation. The `Response` key contained in this item corresponds to the "Response Data" return value.

#### `GetTableInfo`
Returns info about the table; this info is retrieved from AWS.

```lua
local tableInfo = table:GetTableInfo()
```

??? Success "Response Structure"
    ```JSON
    { 
        "ArchivalSummary": { 
            "ArchivalBackupArn": "string",
            "ArchivalDateTime": number,
            "ArchivalReason": "string"
        },
        "AttributeDefinitions": [ 
            { 
                "AttributeName": "string",
                "AttributeType": "string"
            }
        ],
        "BillingModeSummary": { 
            "BillingMode": "string",
            "LastUpdateToPayPerRequestDateTime": number
        },
        "CreationDateTime": number,
        "GlobalSecondaryIndexes": [ 
            { 
                "Backfilling": boolean,
                "IndexArn": "string",
                "IndexName": "string",
                "IndexSizeBytes": number,
                "IndexStatus": "string",
                "ItemCount": number,
                "KeySchema": [ 
                { 
                    "AttributeName": "string",
                    "KeyType": "string"
                }
                ],
                "Projection": { 
                "NonKeyAttributes": [ "string" ],
                "ProjectionType": "string"
                },
                "ProvisionedThroughput": { 
                "LastDecreaseDateTime": number,
                "LastIncreaseDateTime": number,
                "NumberOfDecreasesToday": number,
                "ReadCapacityUnits": number,
                "WriteCapacityUnits": number
                }
            }
        ],
        "GlobalTableVersion": "string",
        "ItemCount": number,
        "KeySchema": [ 
            { 
                "AttributeName": "string",
                "KeyType": "string"
            }
        ],
        "LatestStreamArn": "string",
        "LatestStreamLabel": "string",
        "LocalSecondaryIndexes": [ 
            { 
                "IndexArn": "string",
                "IndexName": "string",
                "IndexSizeBytes": number,
                "ItemCount": number,
                "KeySchema": [ 
                { 
                    "AttributeName": "string",
                    "KeyType": "string"
                }
                ],
                "Projection": { 
                "NonKeyAttributes": [ "string" ],
                "ProjectionType": "string"
                }
            }
        ],
        "ProvisionedThroughput": { 
            "LastDecreaseDateTime": number,
            "LastIncreaseDateTime": number,
            "NumberOfDecreasesToday": number,
            "ReadCapacityUnits": number,
            "WriteCapacityUnits": number
        },
        "Replicas": [ 
            { 
                "GlobalSecondaryIndexes": [ 
                { 
                    "IndexName": "string",
                    "ProvisionedThroughputOverride": { 
                        "ReadCapacityUnits": number
                    }
                }
                ],
                "KMSMasterKeyId": "string",
                "ProvisionedThroughputOverride": { 
                "ReadCapacityUnits": number
                },
                "RegionName": "string",
                "ReplicaStatus": "string",
                "ReplicaStatusDescription": "string",
                "ReplicaStatusPercentProgress": "string"
            }
        ],
        "RestoreSummary": { 
            "RestoreDateTime": number,
            "RestoreInProgress": boolean,
            "SourceBackupArn": "string",
            "SourceTableArn": "string"
        },
        "SSEDescription": { 
            "InaccessibleEncryptionDateTime": number,
            "KMSMasterKeyArn": "string",
            "SSEType": "string",
            "Status": "string"
        },
        "StreamSpecification": { 
            "StreamEnabled": boolean,
            "StreamViewType": "string"
        },
        "TableArn": "string",
        "TableId": "string",
        "TableName": "string",
        "TableSizeBytes": number,
        "TableStatus": "string"
    }
    ```
