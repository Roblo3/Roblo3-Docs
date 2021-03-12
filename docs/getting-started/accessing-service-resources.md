# Accessing Service Resources
Using the Roblo3 SDK to access built-in AWS Service Resources is simple. Since the SDK should only ever be accessed by the game server, place the Roblo3 SDK into `ServerScriptService` or `ServerStorage`, then call require on the path to the SDK.

!!! Note
    For all examples shown in this documentation, it is assumed that the SDK has been placed in the root directory of `ServerScriptService`. Additionally, although `game.ServerScriptService` and `game:GetService("ServerScriptService")` can function the same, all examples will utilize `game:GetService` to make references to any Roblox game service.

As an example, let's get the Amazon Resource Name (ARN) of the `TestTable` DynamoDB table in the `us-west-2 (Oregon)` region via the service resource. First, we need to require the SDK:

```lua
local ServerScriptService = game:GetService("ServerScriptService")
local roblo3 = require(ServerScriptService.Roblo3)
```

Next, to access the DynamoDB Service resource, we call the `resource` function of the Roblo3 SDK with the name of our desired service, our Access Key ID, our Secret Access Key, and a dictionary of any additional "keyword" arguments we'd like to pass in; since we want to access the `us-west-2 (Oregon)` region, we'll pass that into our keyword arguments:

```lua
local awsArgs = {
    ["accessKeyId"] = "ACCESS_KEY_ID",
    ["secretAccessKey"] = "SECRET_ACCESS_KEY",
    ["regionName"] = "us-west-2"
}

local dynamodb = roblo3.resource("dynamodb", awsArgs)
```

!!! Note "Default Region"
    By default, the Roblo3 SDK assumes you want to access a resource in the `us-east-1 (N. Virgina)` service region. This is because the default region for AWS is `us-east-1 (N. Virginia)`.

!!! Note "Security Credentials"
    Upon initialization of a resource, the Roblo3 SDK will automatically use the Access Key ID and Secret Access Key stored in the environment under `accessKeyId` and `secretAccessKey` respectively, if any are found. However, supplying these arguments into the resource function will override whatever is in the environment variables.

!!! Warning 
    If the `resource` function doesn't find an Access Key ID or Secret Access Key in the environment **and** one hasn't been supplied in to the `resource` function as keyword arguments, the `resource` function **will** throw an error.

Next, we need to get the `TestTable` DynamoDB object so we can start using its internal functions. We do this by calling the `Table` function from DynamoDB and passing in the name of the DynamoDB table we want to access; in this case, we'll pass in "TestTable":

```lua
local testTable = dynamodb:Table("TestTable")
```

After this, we now have a `TestTable` DynamoDB object we can interact with. Since we wanted to get the ARN of the table, we first need to call `DescribeTable` on our table object to get its info from AWS:

```lua
local tableInfo = TestTable:DescribeTable()
```

The `DescribeTable` function returns a dictionary of key-value pairs we can use to access our data. In our case, since we want the ARN of the table, we'll access the `TableArn` key (which, in our case, is stored in the top-most level of the dictionary) since this is how AWS returns the data to us:

```lua
local tableArn = tableInfo["TableArn"]
```

Now we can do whatever we want with the table ARN, such as print it to the server console:

```lua
print(tableArn)
```

All-in-all, our code looks like this:

```lua
local ServerScriptService = game:GetService("ServerScriptService")
local roblo3 = require(ServerScriptService.Roblo3)

local awsArgs = {
    ["accessKeyID"] = "ACCESS_KEY_ID",
    ["secretAccessKey"] = "SECRET_ACCESS_KEY",
    ["regionName"] = "us-west-2"
}

local dynamodb = roblo3.resource("dynamodb", awsArgs)

local TestTable = dynamodb:Table("TestTable")

local tableInfo = TestTable:GetTableInfo()
local tableArn = tableInfo.TableArn

print(tableArn)

-- Prints something like:
-- arn:aws:dynamodb:us-west-2:123456789012:table/TestTable
```
