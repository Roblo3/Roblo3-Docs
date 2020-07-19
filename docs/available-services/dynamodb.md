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
    | [`DeleteItem`](#deleteitem) | Deletes the given item from the DynamoDB table |
    | [`GetItem`](#getitem) | Gets the given item from the DynamoDB table |
    | [`GetTableInfo`](#gettableinfo) | Gets info about the table from AWS |
    | [`PutItem`](#putitem) | Either creates or replaces the given item in the DynamoDB table |
    | [`UpdateItem`](#updateitem) | Updates the given item in the DynamoDB table |

#### `DeleteItem`
Deletes the requested item, based on the key provided.

```lua
local response = table:DeleteItem(
    ["Key"] = {
        ["key name"] = "key value"
    }
)
```

???+ Bug "`ReturnItemCollectionMetrics` Returns Nothing"
    During testing, it was found that `ReturnItemCollectionMetrics` does not return any info despite being set to `SIZE` and sent with the payload body; this situation occurred when testing in both Roblox Studio and Postman. As such, this appears to be a limitation of Amazon Web Services. Do not rely upon this metric being returned from DynamoDB.


??? Info "Parameters"
    `DeleteItem` does not take parameters in normal Lua fashion. Instead, it takes a dictionary of key-value pairs and parses them to its arguments. The values shown below are parsed:
    
    | Parameter Name | Type       | Required | Description |
    | -------------- | ---------- | -------- | ----------- |
    | Key            | Dictionary | Yes      | The key-value pair corresponding to the key provided when you created the DynamoDB table. If you specified a simple key, only the primary key should be provided. If you specified a composite key, both the primary key and sort key must be provided. |
    | ConditionExpression | String | No | A condition that must be satisfied, otherwise the update will fail gracefully. See the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#DDB-DeleteItem-request-ConditionExpression) and [AWS DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.ConditionExpressions.html) for more information. |
    | ExpressionAttributeNames | Dictionary | No | A dictionary of substitution tokens for names within an expression. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#DDB-DeleteItem-request-ExpressionAttributeNames). |
    | ExpressionAttributeValues | Dictionary | No | A dictionary of values that will be substitued in an expression. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#DDB-DeleteItem-request-ExpressionAttributeValues). |
    | ReturnConsumedCapacity | String | No | Determines the amount of detail about used provisioned capacity that will be returned by DynamoDB. The Roblo3 SDK defaults this to "TOTAL". For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#DDB-DeleteItem-request-ReturnConsumedCapacity). |
    | ReturnItemCollectionMetrics | String | No | Determines the metrics about the item that are returned by DynamoDB. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#DDB-DeleteItem-request-ReturnItemCollectionMetrics). **Note:** During testing in both Roblox Studio and Postman, this parameter was found to return nothing from DynamoDB. Do not rely upon this metric being returned from DynamoDB. |
    | ReturnValues | String | No | Determines what values about the specified item prior to being changed by `PutItem`, if any are being returned. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html#DDB-DeleteItem-request-ReturnValues). |
   

??? Success "Return Values"
    | Return Name | Description |
    | ----------- | ----------- |
    | Response Data | A Lua table containing the metrics specified in the parameters that are to be returned. This has been parsed and translated from DynamoDB JSON to regular Lua table. |
    | Body | A Lua table that contains the original body from the request, ran through a regular JSON parser but not the DynamoDB JSON parser. |
    | Raw Response | A Lua table of the raw, semi-parsed response from the `Requests` handler. This contains extra data that the `Requests` handler uses to determine automatic-retry and error propagation. |

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
    | ConsistentRead | Boolean    | No       | Determines whether or not *strongly* consitent reads are used; if this is not specified or set to false, *eventually* consitent reads are used. |
    | ExpressionAttributeNames | Dictionary | No | A dictionary of substitution tokens for names within an expression. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ExpressionAttributeNames). |
    | ProjectionExpression | String | No | A string indicating which attributes to retrieve from the table. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html#DDB-GetItem-request-ProjectionExpression). |
    | ReturnConsumedCapacity | String | No | Determines the amount of detail about used provisioned capacity that will be returned by DynamoDB. The Roblo3 SDK defaults this to "TOTAL". For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ReturnConsumedCapacity). |

??? Success "Return Value"
    | Return Name | Description |
    | ----------- | ----------- |
    | Item        | A Lua table of the requested item, or `nil` if DynamoDB didn't return an item (i.e., there wasn't an item that matched the key provided).
    | Body | A Lua table that contains the original body from the request, ran through a regular JSON parser but not the DynamoDB JSON parser.
    | Raw Response  | A Lua table of the raw, semi-parsed response from the `Requests` handler. This contains extra data that the `Requests` handler uses to determine automatic-retry and error propagation.

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

#### `PutItem`
Creates **or** replaces the item with the given key and values.

```lua
local response = table:PutItem(
    ["Item"] = {
        ["name"] = "value",
        ["new"] = "values",
        ["are"] = {
            ["replaced"] = "when",
            ["this"] = {
                "is",
                "called"
            }
        }
    }
)
```

???+ Bug "`ReturnItemCollectionMetrics` Returns Nothing"
    During testing, it was found that `ReturnItemCollectionMetrics` does not return any info despite being set to `SIZE` and sent with the payload body; this situation occurred when testing in both Roblox Studio and Postman. As such, this appears to be a limitation of Amazon Web Services. Do not rely upon this metric being returned from DynamoDB.

??? Info "Parameters"
    `PutItem` does not take parameters in normal Lua fashion. Instead, it takes a dictionary of key-value pairs and parses them to its arguments. The values shown below are parsed:

    | Parameter Name | Type | Required | Description |
    | --------------- | ---- | -------- | ----------- |
    | Item | Dictionary | Yes | The key-value pairs that correspond to desired item's attributes. Note that your partition key must be included in this dictionary; not doing so **will** throw an error when the SDK receives a response from DynamoDB. |
    | ConditionExpression | String | No | A condition that must be satisfied, otherwise the update will fail gracefully. See the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#DDB-UpdateItem-request-ConditionExpression) and [AWS DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.ConditionExpressions.html) for more information. |
    | ExpressionAttributeNames | Dictionary | No | A dictionary of substitution tokens for names within an expression. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ExpressionAttributeNames). |
    | ExpressionAttributeValues | Dictionary | No | A dictionary of values that will be substitued in an expression. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ExpressionAttributeValues). |
    | ReturnConsumedCapacity | String | No | Determines the amount of detail about used provisioned capacity that will be returned by DynamoDB. The Roblo3 SDK defaults this to "TOTAL". For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ReturnConsumedCapacity). |
    | ReturnItemCollectionMetrics | String | No | Determines the metrics about the item that are returned by DynamoDB. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ReturnItemCollectionMetrics). **Note:** During testing in both Roblox Studio and Postman, this parameter was found to return nothing from DynamoDB. Do not rely upon this metric being returned from DynamoDB. |
    | ReturnValues | String | No | Determines what values about the specified item prior to being changed by `PutItem`, if any are being returned. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html#DDB-PutItem-request-ReturnValues). |

??? Success "Return Values"
    | Return Name | Description |
    | ----------- | ----------- |
    | Response Data | A Lua table containing the metrics specified in the parameters that are to be returned. This has been parsed and translated from DynamoDB JSON to regular Lua table. |
    | Body | A Lua table that contains the original body from the request, ran through a regular JSON parser but not the DynamoDB JSON parser. |
    | Raw Response | A Lua table of the raw, semi-parsed response from the `Requests` handler. This contains extra data that the `Requests` handler uses to determine automatic-retry and error propagation. |

#### `UpdateItem`
Updates the requested item, based on the key provided.

```lua
local response = table:UpdateItem(
    ["Key"] = {
        ["key name"] = "key value"
    },
    ["UpdateExpression"] = "set attribute1=:a, attribute2=:x, attribute3=:y, attribute4=:z",
    ["ExpressionAttributeValues"] = {
        [":a"] = "new value",
        [":x"] = "new value",
        [":y"] = {
            "new",
            "values"
        },
        [":z"] = {
            ["new"] = "keys",
            ["and"] = "values"
        }
    }
)
```

???+ Bug "`ReturnItemCollectionMetrics` Returns Nothing"
    During testing, it was found that `ReturnItemCollectionMetrics` does not return any info despite being set to `SIZE` and sent with the payload body; this situation occurred when testing in both Roblox Studio and Postman. As such, this appears to be a limitation of Amazon Web Services. Do not rely upon this metric being returned from DynamoDB.

??? Info "Parameters"
    `UpdateItem` does not take parameters in normal Lua fashion. Instead, it takes a dictionary of key-value pairs and parses them to its arguments. The values shown below are parsed:

    | Parameter Name | Type       | Required | Description |
    | -------------- | ---------- | -------- | ----------- |
    | Key            | Dictionary | Yes      | The key-value pair corresponding to the key provided when you created the DynamoDB table. If you specified a simple key, only the primary key should be provided. If you specified a composite key, both the primary key and sort key must be provided. |
    | ConditionExpression | String | No | A condition that must be satisfied, otherwise the update will fail gracefully. See the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#DDB-UpdateItem-request-ConditionExpression) and [AWS DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.ConditionExpressions.html) for more information. |
    | ExpressionAttributeNames | Dictionary | No | A dictionary of substitution tokens for names within an expression. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#DDB-UpdateItem-request-ExpressionAttributeNames). |
    | ExpressionAttributeValues | Dictionary | No | A dictionary of values that will be substitued in an expression. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#DDB-UpdateItem-request-ExpressionAttributeValues). |
    | ReturnConsumedCapacity | String | No | Determines the amount of detail about used provisioned capacity that will be returned by DynamoDB. The Roblo3 SDK defaults this to "TOTAL". For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#DDB-UpdateItem-request-ReturnConsumedCapacity). |
    | ReturnItemCollectionMetrics | String | No | Determines the metrics about the item that are returned by DynamoDB. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#DDB-UpdateItem-request-ReturnItemCollectionMetrics). **Note:** During testing in both Roblox Studio and Postman, this parameter was found to return nothing from DynamoDB. Do not rely upon this metric being returned from DynamoDB. |
    | ReturnValues | String | No | Determines what values about the specified item prior to being changed by `PutItem`, if any are being returned. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#DDB-UpdateItem-request-ReturnValues). |
    | UpdateExpression | String | No | Defines the attributes that are going to be updated. For more information, see the [AWS DynamoDB API Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html#DDB-UpdateItem-request-UpdateExpression). |


??? Success "Return Values"
    | Return Name | Description |
    | ----------- | ----------- |
    | Response Data | A Lua table containing the metrics specified in the parameters that are to be returned. This has been parsed and translated from DynamoDB JSON to regular Lua table. |
    | Body | A Lua table that contains the original body from the request, ran through a regular JSON parser but not the DynamoDB JSON parser. |
    | Raw Response | A Lua table of the raw, semi-parsed response from the `Requests` handler. This contains extra data that the `Requests` handler uses to determine automatic-retry and error propagation. |
