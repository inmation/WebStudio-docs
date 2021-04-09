# Actions

Pipeline can consist of actions with `type`:

- [action](#action): Refers to another action to be executed.
- [collect](#collect): Collect data from a widget.
- [consoleLog](#consolelog): Write to the browser's console log.
- [convert](#convert): Converts data to and from JSON, Base64.
- [copy](#copy): Copy to clipboard.
- [function](#function): Advanced Endpoint call to the system.
- [modify](#modify): Change the model of a widget.
- [notify](#notify): Display a notification.
- [openLink](#openLink): Opens the a URL in the browser.
- [passthrough](#passthrough): Just passes the input message to the next action with the option to merge a message / payload. **Default**
- [prompt](#prompt): Show a dialog.
- [read](#read): Reads a value of an object.
- [read-write](#read-write): Used for data sources, supports `read` and `write`.
- [refresh](#refresh): Refresh a widget.
- [send](#send): Send data to another widget.
- [switch](#switch): Execute different actions based on conditions.
- [transform](#transform): Transform the data using e.g. MongoDB's Aggregation Pipeline logic.
- [wait](#wait): Adds a delay before executing the next action.
- [write](#write): Writes a value to an object.

Note: Features marked with (*) are not supported yet.

## Supported Action Types

### Action

Points to an action in the widget own `actions` collection or in the grid `actions` collection. The lookup of action names is dominant for the one which are in the same component. So if a widget refers to an action name which exists in his own and in the grid collection, the one in the widget collection will be executed.

```jsonc
{
    "type": "action",
    "name": "NAME OF THE ACTION"
}
```

### Collect

Collect data from a widget. The data which is retrieved is dependent per widget.
The data will be set on the provided `key` name in the message `payload` to the next action. If no `key` is specified the whole `payload` will be overwritten with the collected data.

```json
{
    "type": "collect",
    "from": "Place here the ID of the widget",
    "key": "collectedData"
}
```

`from` can have the value `self` in case the pipeline needs to fetch data from its own widget.

### Console Log

Writes the input message `payload` to the browser's console log.

```json
{
    "type": "consoleLog"
}
```

Add context by providing a `tag`.

```json
{
    "type": "consoleLog",
    "tag": "fetch response"
}
```

Example to set a fixed text message on the `payload`:

```json
{
    "type": "consoleLog",
    "tag": "fetch response",
    "message": {
        "payload": "This is just a message"
    }
}
```

Example to merge the input message `payload` with other key-value data.

```json
{
    "type": "consoleLog",
    "tag": "fetch response",
    "message": {
        "payload": {
            "otherTest": "This is just some other text message"
        }
    }
}
```

### Copy

Copies the `payload` to the clipboard.

```json
{
    "type": "copy"
}
```

### Convert

Converts the `payload` to a specified format. Supported formats are:

- `json`
- `base64`

Encode the payload:

```json
{
    "type": "convert",
    "encode": "json"
}
```

Decode the payload:

```json
{
    "type": "convert",
    "decode": "json"
}
```

### Function

To invoke an Advanced Endpoint.

```jsonc
{
    "type": "function",
    "lib": "LIBRARY NAME",
    "func": "FUNCTION NAME", // Optional function name in case library is Lua table.
    "farg": {}, // Optional function argument.
    "ctx": "" // Optional system object path.
}
```

### Modify

With `modify` the model of a widget can be changed. If no modification operator is specified a `mergeObjects` will be performed. This will merge / overwrite the properties from the message `payload` into the model.

Supported modification operators in order of execution:

- `set`: Add field to model or update field.
- `unset`: Remove field from model.
- `addToArray`: Adds an item to an array field.
- `removeFromArray`: Removes one or more items from an array that matches the provided fields.
- `filter`: Removes items from an array field by condition.

A `transform` action will be performed under the hood with `completeMsgObject` set to true. During execution the `model` field in the input message will always be set with the model of the designated widget. The message `payload` can be used to set values dynamically.

Main signature of the `modify` action is:

```jsonc
{
    "type": "modify",
    "id": "TextWidget",
    "refresh": true, // Default is true, in case you don't want to let the widget perform a refresh, set if to false.
    "debug": false //  If true, writes the model after the modification to the console log.
}
```

Merge the widget model with the message `payload`:

```json
{
    "type": "modify",
    "id": "TextWidget"
}
```

Will result in a `transform` action:

```json
{
    "type": "transform",
    "completeMsgObject": true,
    "aggregateOne": [
        {
            "$project": {
                "model": {
                    "$mergeObjects": [
                        "$model",
                        "$payload"
                    ]
                },
                "payload": "$payload"
            }
        }
    ]
}
```

To modify a specific sub document of the model, make use of the supported modification operators.

Example to update the font size:

```json
{
    "type": "modify",
    "id": "TextWidget",
    "set": [
        {
            "name": "model.options.style.fontSize",
            "value": "50px"
        },
        {
            "name": "model.options.style.fontFamily",
            "value": "Courier New"
        }
    ]
}
```

Will result in a `transform` action:

```json
{
    "type": "transform",
    "aggregateOne": [
        {
            "$set": {
                "model.options.style.fontSize": "50px",
                "model.options.style.fontFamily": "Courier New"
            }
        }
    ]
}
```

Example to update the style:

With this example the custom style will only include `fontSize`.

```json
{
    "type": "modify",
    "id": "TextWidget",
    "set": [
        {
            "name": "model.options.style",
            "value": {
                "fontSize": "50px"
            }
        }
    ]
}
```

Will result in a `transform` action:

```json
{
    "type": "transform",
    "aggregateOne": [
        {
            "$set": {
                "model.options.style": {
                    "fontSize": "50px"
                }
            }
        }
    ]
}
```

Example to remove the style example:

This way the widget uses the default style.

```json
{
    "type": "modify",
    "id": "TextWidget",
    "unset": ["model.options.style"]
}
```

Will result in a `transform` action:

```json
{
    "type": "transform",
    "aggregateOne": [
        {
            "$project": {
                "model.options.style": 0
            }
        }
    ]
}
```

Example to update an item by index:

This example changes the `aggregate` of the first pen in a chart. Since the field notation is one-on-one used in the Aggregation Pipeline, the index is zero based.

```json
{
    "type": "modify",
    "id": "chartWidget",
    "set": [
        {
            "name": "model.chart.pens.0.aggregate",
            "value": "AGG_TYPE_AVERAGE"
        }
    ]
}
```

Will result in a `transform` action:

```json
{
    "type": "transform",
    "aggregateOne": [
        {
            "$set": {
                "model.chart.pens.0.aggregate": "AGG_TYPE_AVERAGE"
            }
        }
    ]
}
```

Example to add an item to the `data` array in the `dataSource` of a table widget:

```json
{
    "type": "modify",
    "id": "fixedTableWidget",
    "addToArray": [
        {
            "name": "model.dataSource.data",
            "value": {
                "column1": 1,
                "column2": 2
            }
        }
    ]
}
```

Will result in a `transform` action:

```json
{
    "type": "transform",
    "aggregateOne": [
        {
            "$set": {
                "model.dataSource.data": {
                    "$concatArrays": [
                        {
                            "$cond": [
                                {
                                    "$isArray": [
                                        "$model.dataSource.data"
                                    ]
                                },
                                "$model.dataSource.data",
                                []
                            ]
                        },
                        [
                            {
                                "column1": 1,
                                "column2": 2
                            }
                        ]
                    ]
                }
            }
        }
    ]
}
```

Example to remove an item from an array by index:

Removes the second row from a fixed data table.

```json
{
    "type": "modify",
    "id": "fixedTableWidget",
    "removeFromArray": [
        {
            "name": "model.dataSource.data",
            "idx": 2
        }
    ]
}
```

Will result in a `transform` action:

```json
{
    "type": "transform",
    "aggregateOne": [
        {
            "$project": {
                "model.dataSource.data.1": 0
            }
        }
    ]
}
```

Example to remove an item from an array based on a match expression:

Removes the rows from a fixed data table of which `name` contains value `Inside Temperature` and `value` 26.

```json
{
    "type": "modify",
    "id": "fixedTableWidget",
    "removeFromArray": [
        {
            "name": "model.dataSource.data",
            "item": {
                "name": "Inside Temperature",
                "value": 26
            }
        }
    ]
}
```

Alternative structure:

```json
{
    "type": "modify",
    "id": "fixedTableWidget",
    "removeFromArray": [
        {
            "name": "model.dataSource.data",
            "item": [
                {
                    "name": "name",
                    "value": "Inside Temperature"
                },
                {
                    "name": "value",
                    "value": 26
                }
            ]
        }
    ]
}
```

Will result in a `transform` action:

```json
{
    "type": "transform",
    "aggregateOne": [
        {
            "$set": {
                "dataSource.data": {
                    "$filter": {
                        "input": "$model.dataSource.data",
                        "as": "item",
                        "cond": {
                            "$and": [
                                {
                                    "$ne": [
                                        "$$item.name",
                                        "Inside Temperature"
                                    ]
                                },
                                {
                                    "$ne": [
                                        "$$item.value",
                                        26
                                    ]
                                }
                            ]
                        }
                    }
                }
            }
        }
    ]
}
```

Example to filter items from an array:

Filters the rows from a fixed data table of which the column `value` is greater than or equal to 20. The `condition` is an Aggregation Pipeline filter condition. Within the condition `$$item` is reserved for referencing an item in the array.

```json
{
    "type": "modify",
    "id": "fixedTableWidget",
    "filter": [
        {
            "name": "model.dataSource.data",
            "condition": {
                "$gte" : [
                    "$$item.value", 20
                ]
            }
        }
    ]
}
```

Will result in a `transform` action:

```json
{
    "type": "transform",
    "aggregateOne": [
        {
            "$set": {
                "dataSource.data": {
                    "$filter": {
                        "input": "$model.dataSource.data",
                        "as": "item",
                        "cond": {
                            "$gte" : [
                                "$$item.value", 20
                            ]
                        }
                    }
                }
            }
        }
    ]
}
```

### Notify

Displays a notification on the top right of WebStudio.

```jsonc
{
    "type": "notify",
    "title": "Copied",
    "text": "📋 Copied to Clipboard",
    "duration": 2500,
    "transition": "slide"
}
```

`title` is optional and by default the `title` defined in the `captionBar` in the widget model. In case the `title` is not defined in the caption bar the `name` or `id` of the widget will be shown.

`duration` is optional and by default `3000`.

`transition` is optional and by default `slide`.

- `slide`  Smooth sliding of notification
- `bounce` Bounce in of the notification
- `zoom` Zoom in and out
- `flip` Flips the notification

### Open Link

Opens a hypermedia link.

```json
{
    "type": "openLink",
    "url": "https://www.lipsum.com",
    "target": "_blank"
}
```

`target` is optional and by default `_blank`.

- `_self`  Opens the document in the same window/tab as it was clicked.
- `_blank` Opens the document in a new window or tab.

### Passthrough

The input message will be passed through the next action with the option to merge with a defined message.

```json
{
    "type": "passthrough",
    "message": {
        "someField": "This field will be merged besides input message topic and payload",
        "payload": {
            "attrib": "This field will be merged within the input message payload"
        }
    }
}
```

### Prompt (Dialog)

This action which show (prompt) a dialog which content is a widget model.

Example to prompt a text widget.

```json
 {
    "type": "prompt",
    "width": "500px",
    "height": "500px",
    "message": {
        "payload": {
            "type": "text",
            "text": "Hello world"
        }
    }
}
```

- `content`: Can be a single widget or a complete grid compilation. The latter is not supported yet.

### Read

Reads the dynamic value of an object or the value of an object property by executing a Read endpoint request.

Example read object's dynamic value.

```json
{
    "type": "read",
    "path": "/System/Core/Examples/Demo Data/Process Data/FC4711"
}
```

Example read object's property value.

```json
{
    "type": "read",
    "path": "/System/Core/Examples/Demo Data/Process Data/FC4711.OPCEngUnit"
}
```

Example to read a Lua KPI table object.

```json
{
    "type": "read",
    "path": "/System/Core/Examples/Demo Data/Process Data/FC4711",
    "opt": {
        "STARTTIME": "2020-10-13T00:00:00.000Z",
        "ENDTIME": "2020-10-14T00:00:00.000Z",
        "TIMESTAMP": "1602633600"
    }
}
```

Example of the query mechanism support by the Read Web API endpoint.

```json
{
    "type": "read",
    "path": "/System/Core/Examples/Demo Data/Process Data/DC4711",
    "opt": {
        "q": "COREPATH"
    }
}
```

- `opt`: optional field, supported arguments are described in the Read endpoint section in the Web API documentation.

### Read-write

Can be used for widgets which support reading and writing.

```json
{
    "dataSource": {
        "type": "read-write",
        "path": "/System/Core/Examples/Tables/Example01"
    }
}
```

### Refresh

Refresh a widget. No message will be send to the target widget. In case you want to send a message to a widget, use a `send` action with the message `topic` set to `refresh`.

```json
{
    "type": "refresh",
    "id": "Place here the ID of the widget"
}
```

The `to` field can have the value `self` in case the action pipeline needs to refresh its own widget.

### Send

Widgets data exchange

Sending message from one to another widget can be done using the `send` action. This action does not change the output message. The output message is the same as the input message.

```json
{
    "type": "send",
    "to": "Place here the ID of the widget"
}
```

The `to` field can have the value `self` in case the pipeline needs to send data to its own widget. The update behavior differs per widget.

Supported topics:

- `refresh`: the recipient widget will perform a refresh. **(default)**
- `do`: the receiving widget will perform the action or action pipeline, which is defined on the message. (*)
- `update`: the receiving widget will perform an update. This will bypass the data source action.

#### Refresh Topic

The recipient widget will perform a refresh. The recipient widget will execute the data source action (pipeline) with the provided message `payload`. The [refresh life cycle](../widgets/README.md#refresh-life-cycle-hooks) will be performed including a fetch if a data source is present.

Example to send a message to another component to refresh itself:

```jsonc
{
    "type" : "send",
    "to" : "Place here the ID of the widget",
    "message": {
        "topic": "refresh" // Can be omitted because it is default.
        "payload": {} // Can be any type of value. Typically an object is ued.
    }
}
```

#### Do Topic (*)

Example to send a message to another component to execute a certain action:

```jsonc
{
    "type" : "send",
    "to" : "Place here the ID of the widget",
    "message": {
        "topic": "do",
        "action" : [  // Can be a single action or action pipeline.
            {
                "type" : "action",
                "name" : "NAME OF ACTION TO EXECUTE"
            }
        ]
    }
}
```

#### Update Topic

The recipient widget will perform an update. The recipient widget only update its known properties with the provided message `payload`. The [update life cycle](../widgets/README.md#update-life-cycle-hooks) will be performed.

```jsonc
{
    "type" : "send",
    "to" : "Place here the ID of the widget",
    "message": {
        "topic": "update",
        "payload": {} // Can be any type of value. Typically an object is ued.
    }
}
```

### Switch

Execute actions based on rules. A rule will be checked by performing a `queryOne` transformation. In case the result of the queryOne transformation is something other than `null` the action(s) defined in the `case` statement will be executed. If one rule matches, its `action` will be executed and further testing of the subsequent rules with be stopped.

In case `checkAll` is set to `true`, the 'initial' input message of the switch will be passed to each action pipeline of the matched rules. The output message of the last executed action pipeline, will be the output message of this switch action. When no rule matches, the output of the switch action is the same as the input.

```jsonc
{
    "type": "switch",
    "checkAll": false,
    "case": [
        {
            "match" : {
                "temp": 10
            },
            "action" : { // Can be a single action or action pipeline.
                "type": "action",
                "name": "doSomething"
            }
        },
        {
            "match" : {
                "temp": {
                    "$gte": 20
                }
            },
            "action" : [ // Can be a single action or action pipeline.
                {
                    "type": "action",
                    "name": "doSomethingFirst"
                },
                {
                    "type": "action",
                    "name": "doSomethingExtra"
                }
            ]
        }
    ]
}
```

In order to have a default action in case none of the rules matches, add a rule at the end of the `case` array with an empty object as `match`.

```jsonc
{
    "match" : {},
    "action" : [] // Can be a single action or action pipeline.
}
```

### Transform

Transformation of the data can be performed by the use of the MongoDB Aggregation framework. The input data is normally the value of the `payload` field of the input message. In case the whole message object needs to be available to the transform logic you can set `completeMsgObject` to `true`.

An addition to the MongoDB Aggregation framework, in order to result in a single object the key `aggregateOne` instead of `aggregate` can be used.

Besides `aggregate` and `aggregateOne` also `query` and `queryOne` can be used. The two latter are purely for filtering where `queryOne` returns the first matching object.

MongoDB Documentation:

- [MongoDB Aggregation Pipeline](https://docs.mongodb.com/manual/core/aggregation-pipeline/)
- [MongoDB Query](https://docs.mongodb.com/manual/reference/operator/query/)

#### Transform Example 01

This example is to transform the data to an object with a different key.

- Input message payload:

```json
{
    "name": "Company A",
    "location": "Eindhoven"
}
```

- Transform logic:

```json
{
    "type": "transform",
    "aggregateOne": [
        {
            "$project": {
                "company": "$name"
            }
        }
    ]
}
```

Output message payload:

```json
{
    "company": "Company A"
}
```

Example to search for the object which 'location' has the value 'Cologne':

- Input message payload:

```json
[
    {
        "name": "Company A",
        "location": "Cologne"
    },
    {
        "name": "Company B",
        "location": "Eindhoven"
    }
]
```

- Transform logic:

```json
{
    "type": "transform",
    "aggregate": [
        {
            "$match": {
                "location": "Cologne"
            }
        }
    ]
}
```

- Output message payload:

```json
[
    {
        "name": "Company A",
        "location": "Cologne"
    }
]
```

#### Transform Example 02

This example is to search for the object which 'location' has the value 'Cologne' using a `query`.

- Input message payload:

```json
[
    {
        "name": "Company A",
        "location": "Cologne"
    },
    {
        "name": "Company B",
        "location": "Eindhoven"
    },
    {
        "name": "Company C",
        "location": "Cologne"
    }
]
```

- Transform logic:

```json
{
    "type": "transform",
    "query": {
        "location": "Cologne"
    }
}
```

- Output message payload:

```json
[
    {
        "name": "Company A",
        "location": "Cologne"
    },
    {
        "name": "Company C",
        "location": "Cologne"
    }
]
```

#### Transform Example 03

This example is to search for the object which 'location' has the value 'Cologne' using a `queryOne`. Because of `queryOne` this example returns only the first match.

- Input message payload:

```json
[
    {
        "name": "Company A",
        "location": "Cologne"
    },
    {
        "name": "Company B",
        "location": "Eindhoven"
    },
    {
        "name": "Company C",
        "location": "Cologne"
    }
]
```

- Transform logic:

```json
{
    "type": "transform",
    "queryOne": {
        "location": "Cologne"
    }
}
```

- Output message payload:

```json
{
    "name": "Company A",
    "location": "Cologne"
}
```

### Wait

Wait before the next actions will be executed. `duration` is in milliseconds.

```json
{
    "type": "wait",
    "duration": 1000
}
```

### Write

Writes a value to an object in the system.

```json
{
    "type": "write",
    "path": "/System/Core/Examples/Variable"
}
```
