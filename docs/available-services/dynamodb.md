# DynamoDB

## Service Resource
An object representing DynamoDB as a high-level resource:

```lua
local roblo3 = require(game:GetService("ServerScriptService").Roblo3)

local dynamodb = roblo3.resource("dynamodb")
```

### `dynamodb.Table`
Returns an object representing the DynamoDB table with the given name:

```lua
local table = dynamodb.Table("name")
```

#### Parameters
| Parameter Name | Type   | Required | Description                    |
| -------------- | ------ | -------- | ------------------------------ |
| `name`         | String | Yes      | The name of the DynamoDB table |

#### Returns
| Return Name                        | Description                                     |
| ---------------------------------- | ------------------------------------------------ |
| [`dynamodb.Table`](#dynamodbtable) | An object representing the DynamoDB table given. |

#### Functions
| Function Name  | Parameters | Description                        |
| -------------- | ---------- | ---------------------------------- |
| [`GetTableInfo`](#gettableinfo) | `None`     | Gets info about the table from AWS |

##### `GetTableInfo`
Returns info about the table; this info is retrieved from AWS.

```lua
local tableInfo = table:GetTableInfo()
```

??? Note "`GetTableInfo` Response Structure"
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