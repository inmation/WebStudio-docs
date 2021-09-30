# Actions

Each action has this basic model. An action receives an input message which depend on the action hook and the previous action in case of an action pipeline. After execution of the action it will output a message which could be altered depending on the action type.

```jsonc
{
    "type": "",        // Type of the action.
    "message": {       // (Optional) To be merged with the input message.
        "topic": "",   // (Optional) Can be a string or null.
        "payload": {}  // (Optional) Can be any number, string, object or array.
    }
}
```

Pipeline can consist of actions with `type`:

- [action](#action): Refers to another action to be executed.
- [collect](#collect): Collect data from a widget.
- [consoleLog](#console-log): Write to the browser's console log.
- [convert](#convert): Converts data to and from JSON, Base64.
- [copy](#copy): Copy to clipboard.
- [delegate](#delegate): delegate the execution context of an action pipeline.
- [gettime](#gettime): Converts relative, ISO UTC and milliseconds since Epoch timestamps.
- [function](#function): Advanced Endpoint call to the system.
- [modify](#modify): Change the model of a widget.
- [notify](#notify): Display a notification.
- [openLink](#open-link): Opens a URL in the browser.
- [passthrough](#passthrough): Passes the input message to the next action with the option to merge a message and/or payload.
- [prompt](#prompt-dialog): Show a dialog.
- [read](#read): Reads a value of an object.
- [read-write](#read-write): Used for data sources, supports `read` and `write`.
- [refresh](#refresh): Refresh a widget.
- [send](#send): Send data to another widget.
- [subscribe](#subscribe): Subscribe to data changes in the system.
- [switch](#switch): Execute different actions based on conditions.
- [transform](#transform): Transform the data using MongoDB's Aggregation Pipeline logic.
- [wait](#wait): Adds a delay before executing the next action.
- [write](#write): Writes a value to an object.

Note: Features marked with (*) are not supported yet.

## Supported Action Types

### Action

Invoke a named action defined in the widget's own `actions` collection or in the `actions` collection at compilation level. Actions defined at the widget level take precedence over those at compilation level. If a widget refers to a named action which exists in his own model and at compilation level, the one in the widget collection will be executed.

```jsonc
{
    "type": "action",
    "name": "NAME OF THE ACTION"
}
```

Refer to the [write-example-01](../../GettingStarted/compilations/actions/write-example-01.json) compilation to see how named actions are defined and used.  

### Collect

Collect data from a widget. The specific data retrieved dependents on the source widget. In general, collect returns the same payload data provided to action pipelines defined directly on the referenced widget. Refer to the widget specific documentation for more details. The collected information is assigned to the provided `key` name of the message `payload`. If a `key` is not specified the whole `payload` will be overwritten with the collected data.

```json
{
    "type": "collect",
    "from": "Place the ID of the widget here",
    "key": "collectedData"
}
```

the `from` field can be set to **"self"**. This allows the pipeline to fetch data from the widget that initiated the actions.

This might seem like an odd thing to do since the message at the beginning of a pipeline will be initialized by the source widget, but as the execution progresses this information may be overwritten. Using the `collect` action on **"self"** provides a way to get back to the original content.

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
            "otherText": "This is just some other text message"
        }
    }
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

### Copy

Copies the `payload` to the clipboard.

```json
{
    "type": "copy"
}
```
### Delegate
Using `tabs` widgets, compilations can be created that contain sub-compilations in each tab. Widgets defined within these cannot directly interact with other widgets in peer level compilations.

The `delegate` action provides a mechanism to address this constraint.

```jsonc
{
    "type": "delegate",
    "action":  []  // Action pipeline to be executed in the new context 
}
```

This action is probably easiest to understand by considering an example.

Suppose we have a `tabs` widget with two tabs, each containing their own `text` widget, **text01** and **text02**. We want to change the text of **text02** when clicking on **text01**. As a first attempt, we might try something like this:

```jsonc
{
    "type": "text",
    "text": "Click Me",
    "id": "text01",
    "actions": {
        "onClick": {
            "type": "send",
            "to": {
                "route": [ // This does not work from text01
                    "tabs01",
                    "tab02",
                    "text02"
                ]
            },
            "message": {
                "payload": "Text on tab 1 was clicked"
            }
        }
    }
}
```

This fails since, from within the **tab01** compilation, there is no widget that resolves to the provided route. The route only makes sense when it is traversed starting at the root compilation.

```
+ Root (compilation)
|
+--+ tabs01 (widget)
   |
   +--+ tab01 (compilation)
   |  |
   |  +--+ text01 (widget)  <-- Action starts here
   | 
   +--+ tab02 (compilation)
      |
      +--+ text02 (widget)
```

To get the correct execution context we need to define the action at either the root compilation level, or in the actions section of the **tabs01** widget. 

>**Note:** The execution **context** refers to the compilation model from which routes and widget ids are resolved.  

A named [action](#action) can be be defined in the root compilation and invoked from **text01**. This works since named actions are resolved by searching upwards in the containment hierarchy. In this case the `onClick` could be defined like so:

```json
{
    "type": "text",
    "text": "Click Me",
    "id": "text01",
    "actions": {
        "onClick": {
            "type": "action",
            "name": "modifyWidgetOnSecondTab"
        }
    }
}
```

with the named action in the root compilation:

```jsonc
{
    "actions": {
        "modifyWidgetOnSecondTab": { // this still does not work !
            "type": "send",
            "to": {
                "route": [
                    "tabs01",
                    "tab02",
                    "text02"
                ]
            },
            "message": {
                "payload": "Text on tab 1 was clicked"
            }
        }
    }
}
```

However, just declaring the named action in the root compilation is not enough. The execution context is not affected by where the action is declared, unless `delegate` is used. 

```json
{
    "actions": {
        "modifyWidgetOnSecondTab": {
            "type": "delegate",
            "action": [
                {
                    "type": "send",
                    "to": {
                        "route": [
                            "tabs01",
                            "tab02",
                            "text02"
                        ]
                    },
                    "message": {
                        "payload": "Text on tab 1 was clicked"
                    }
                }
            ]
        }
    }
}
```

In other words, `delegate` changes the context of the execution pipeline to be at the level where it is defined in the compilation. Using the new context, the pipeline defined in the `action` property is executed. 

When the `delegate` action returns, the context is restored and any subsequent actions will be executed in the context that was there before.

>**Note:** The context also includes the widget that initiated the pipeline, and is referred to as `"self"`. If the `delegate` action is declared at compilation level, the `self` widget will not be set. In the current version, `self` cannot yet be used in `route` expressions of delegated actions. 

### GetTime

The `gettime` action can perform two types of functions:

- Convert a relative time expression to the equivalent ISO UTC time string or an Epoch timestamp. By default the output is an ISO string. If the optional `asEpoch` property is set to true, the output is returned as a number.  
- Convert between ISO UTC string and epoch integer

A time relative expression consists of a `*`, indicating **now**, optionally followed by an offset expression subtracted from, or if required, added to the current time.

The offset is stated as an integer number followed by a time scale unit. The supported time scale units are:  

- ms - millisecond
- s - second
- m - minute
- h - hour
- d - day
- w - week

Here are some examples of relative time expressions:

| Expression | Description |
| --- | --- |
| * | Now |
| *-5d | 5 days ago |
| *+30m | 30 minutes from now |

The table below illustrates the conversions performed by the `gettime` action.

| type | value type | value example | results |
| --- | --| --- | --- |
| Relative | string |*-1d | Now minus one day as an ISO UTC string. <br/>Unless: <br/>`asEpoch` : true <br/>Then the output is the number of milliseconds since January first 1970  |
| ISO UTC | string | 2021-04-28T09:44:35.668Z | 1619603075668 |
| Milliseconds since Epoch | number | 1619603075668 | 2021-04-28T09:44:35.668Z

The JSON below shows an example of `gettime` used to convert relative times to absolute ISO UTC strings:

```json
{
    "type": "gettime",
    "set": [
        {
            "name": "starttime",
            "value": "*-5d"
        },
        {
            "name": "endtime",
            "value": "*-1d"
        }
    ]            
}
```

Resulting message:

```json
{
    "payload": {
        "starttime": "2021-04-24T10:31:04.091Z",
        "endtime": "2021-04-28T10:31:04.092Z"
    }
}
```

This example shows how to convert a relative time to its Epoch equivalent:

```json
{
    "type": "gettime",
    "set": [
        {
            "name": "starttime",
            "value": "*-5d",
            "asEpoch" : true
        }
    ]
}
```

yields this output message:

```json
{
    "payload": {
        "starttime": 1619260264103
    }
}
```

The action can also take the `set` instruction from the message payload to do dynamic conversions.

Action message input:

```json
{
    "payload": {
        "set": [
            {
                "name": "myTimestamp",
                "value": "*-2d"
            }
        ]
    }
}
```

Action:

```jsonc
{
    "type": "gettime"       // Notice it does not contain a set field.
}
```

Result is the message output:

```jsonc
{
    "set": [],                      // Since the action result is merged with the message input the set is still present.
    "myTimestamp": 1630497600000
}
```

### Function

Invoke an Advanced Endpoint.

```jsonc
{
    "type": "function",
    "lib": "LIBRARY NAME",
    "func": "FUNCTION NAME", // (Optional) Function name in case library is Lua table.
    "farg": {},              // (Optional) Function argument.
    "ctx": ""                // (Optional) System object path.
}
```

### Modify

With `modify` the model of a widget can be changed. If no modification operator is specified a `mergeObjects` will be performed. This will merge / overwrite the properties from the message `payload` into the model.

To point to the widget which needs to be modified a `id` field. The value is normally a string with the widget ID of the designated widget. In case you need to point to a single tab of a Tab widget or a widget which is residing within a tab, you need to provide a `route`.

```jsonc
{
    "type": "modify",
    "id": "TextWidget"      // Pointing to a Text widget which has an ID of 'TextWidget`.
}
```

Using a `route` to point to a widget within a tab

```jsonc
{
    "type": "modify",
    "id": {
        "route" : [
            "MyTabs",       // ID of the Tabs widget.
            "Tab01",        // ID of the single tab.
            "TextWidget"    // Pointing to a Text widget which has an ID of 'TextWidget` which.
        ]
    }
}
```

The following modification operators are supported, multiples of which can be specified in the same action. They are evaluated in the order shown. In other words, if multiple modification operators are used, the `set` modification is done first, followed by `unset` and so on.

- `set`: Add field to model or update field.
- `unset`: Remove field from model.
- `addToArray`: Adds an item to an array field.
- `removeFromArray`: Removes one or more items from an array that matches the provided fields.
- `filter`: Removes items from an array field based on a condition.

A `transform` action will be performed under the hood with `completeMsgObject` set to true. The `model` field is added to the input message by the modify action before the `transform` actions starts. It is read from the model of the widget being modified (as designated by the `id` field).

The message `payload`, which is passed down from the originating widget or previous action in the pipeline is typically used as the source for setting model fields.

Main signature of the `modify` action is:

```jsonc
{
    "type": "modify",
    "id": "TextWidget",
    "refresh": true,    // Default is true, if you don't want to let the widget perform a refresh, set it to false.
    "debug": false      //  If true, writes the model after the modification to the console log.
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
    "completeMsgObject": true,
    "aggregateOne": [
        {
            "$set": {
                "model.chart.pens.0.aggregate": "AGG_TYPE_AVERAGE"
            }
        }
    ]
}
```

Example to add an item to the `data` array of a table widget:

```json
{
    "type": "modify",
    "id": "fixedTableWidget",
    "addToArray": [
        {
            "name": "model.data",
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
    "completeMsgObject": true,
    "aggregateOne": [
        {
            "$set": {
                "model.data": {
                    "$concatArrays": [
                        {
                            "$cond": [
                                {
                                    "$isArray": [
                                        "$model.data"
                                    ]
                                },
                                "$model.data",
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
            "name": "model.data",
            "idx": 2
        }
    ]
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
                "model.data.1": 0
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
            "name": "model.data",
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
            "name": "model.data",
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
    "completeMsgObject": true,
    "aggregateOne": [
        {
            "$set": {
                "model.data": {
                    "$filter": {
                        "input": "$model.data",
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
            "name": "model.data",
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
    "completeMsgObject": true,
    "aggregateOne": [
        {
            "$set": {
                "model.data": {
                    "$filter": {
                        "input": "$model.data",
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

**Note:** Due to how the `$filter` pipeline is implemented in the external library, the condition expression can only refer to fields contained within `$$item`. What this means is that external variables are not visible inside the condition expression.

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

`title` is optional and is by default set to the `title` defined in the widget `captionBar`. In case the `title` is not defined in the caption bar the `name` or `id` of the widget will be shown.

`duration` is optional and by default `3000`.

`transition` is optional with a default value of `slide`.

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

The input message will be passed through to the next action with the option to merge with a defined message.

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

This action causes a popup dialog (prompt) to be shown. Its content is a widget model, contained within the payload of the message.

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

- `content`: Can be a single widget or a complete compilation.

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

- `opt`: optional field, supported arguments are described in the Read endpoint section in the [Web API documentation](https://inmation.com/docs/api/latest/webapi/read.html#queries).

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
    "id": "Place the ID of the widget here"
}
```

The `to` field can have the value `self` in case the action pipeline needs to refresh its own widget.

### Send

Widgets data exchange

Sending message from one widget to another can be done using the `send` action. This action does not change the output message. The output message is the same as the input message.

```json
{
    "type": "send",
    "to": "Place the ID of the widget here"
}
```

The value of the `to` field can be set to `self` in case the pipeline needs to send data to its own widget. The update behavior differs per widget.

Using a `route` to point to a widget within a tab

```jsonc
{
    "type": "send",
    "to": {
        "route" : [
            "MyTabs",       // ID of the Tabs widget.
            "Tab01",        // ID of the single tab.
            "TextWidget"    // Pointing to a Text widget which has an ID of 'TextWidget` which.
        ]
    }
}
```

Supported topics:

- `refresh`: the recipient widget will perform a refresh. **(default)**
- `update`: the receiving widget will perform an update. This will bypass the data source action.

#### Refresh Topic

The recipient widget will perform a refresh. It will execute the data source action (pipeline) with the provided message `payload`. The [refresh life cycle](../widgets/README.md#refresh-life-cycle-hooks) will be performed including a fetch if a data source is present.

Example to send a message to another component to refresh itself:

```jsonc
{
    "type" : "send",
    "to" : "Place the ID of the widget here",
    "message": {
        "topic": "refresh" // Can be omitted because it is default.
        "payload": {} // Can be any type of value. Typically an object is used.
    }
}
```

#### Update Topic

The recipient widget will perform an update. It only updates its known properties with the provided message `payload`. The [update life cycle](../widgets/README.md#update-life-cycle-hooks) will be performed.

```jsonc
{
    "type" : "send",
    "to" : "Place the ID of the widget here",
    "message": {
        "topic": "update",
        "payload": {} // Can be any type of value. Typically an object is used.
    }
}
```

### Subscribe

Subscribe to object data changes in the system. Typically used in `dataSource` configurations. If necessary, use the `willUpdate` action hook to transform the data.

```json
{
    "type": "subscribe",
    "path": "/System/Core/Examples/Variable"
}
```

### Switch

Execute actions based on rules. A rule will be checked by performing a [`queryOne`](#transform) transformation. In case the result of the queryOne transformation is something other than `null` the action(s) defined in the `case` statement will be executed. If one rule matches, its `action` will be executed and further testing of the subsequent rules will be stopped.

In case `checkAll` is set to `true`, the 'initial' input message of the switch will be passed to each action pipeline of the matched rules. The output message of the last executed action pipeline, will be the output message of this switch action. When no rule matches, the output of the switch action is the same as the input.

In order to have a **default action** in case none of the rules match, add a extra rule at the end of the `case` array with an empty `match` condition.

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
        },
        {
            "match" : {}, // Default action
            "action" : [] // Can be a single action or action pipeline.
        }
    ]
}
```

### Transform

Transformation of data can be performed by means of the MongoDB Aggregation framework. The input data is normally the value of the `payload` field of the input message. In case the whole message object needs to be available to the transform logic you can set `completeMsgObject` to `true`.

The aggregation pipeline often returns an array of one or more elements. In most cases, pipeline actions which ingest the output of the transformation are however looking for a single object. As a convenience the WebStudio specific `aggregateOne` options can be used instead of `aggregate`. It return the first element from the resulting transformation.

Besides `aggregate` and `aggregateOne`, `query` and `queryOne` can also be used in scenarios where filtering is required. As expected `queryOne` returns the first matching object.

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

Example to search for the object with 'location' value 'Cologne':

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

**Note:** Even though only one element was matched, the returned massage payload is still an array. If `aggregateOne` were used instead of `aggregate`, the output would look like this:

```json
{
    "name": "Company A",
    "location": "Cologne"
}
```

#### Transform Example 02

This example uses a `query` to search for the object with 'location' value of 'Cologne'.

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

This example uses `queryOne` to search for the object with 'location' value 'Cologne'. Like `aggregateOne`, it returns only the first match.  

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

Writes a value to an object in the system. The write action inspects the message payload passed to it, looking for variables `v`, `q` and `t` which it uses to update the object identified by the path.

```json
{
    "type": "write",
    "path": "/System/Core/Examples/Variable"
}
```

#### Input Message

Example:

```json
{
    "payload": {
        "v": 79,
        "t": "2021-08-10T08:34:13.000Z",
        "q": 0
    }
}
```

The value parameter `v` will be a number for most tags and is the only mandatory variable. It may be specified explicitly as shown above, or can be set as the value of the payload itself as shown below. In the latter case, `t` and `q` cannot be set

```json
// Value supplied as the payload
{
    "payload": 79
}
```

If the timestamp field `t` is omitted, the current time is assumed by the core. If present, `t` must be expressed as an ISO UTC string.

It would be rare to supply a quality value `q` explicitly, but the option is available. The parameter can be omitted in most cases in which case 0 (GOOD) is assumed.

#### Return Message

The `write` action return message, which can be passed to a subsequent action in a pipeline, is dependent on which parameters were supplied to it.

For an input message containing only a value:

```json
// Input message
{
    "payload": {
        "v": 79
    }
}
```

the return message looks like this:

```json
// Output message
{
    "payload": 79
}
```

On the other hand, if the value and either the time-stamps or the quality parameters are supplied:

```json
// Input message
{
    "payload": {
        "v": 79,
        "q": 0
    }
}
```

the output will contain all vqt values returned from the `write` call made to the backend:

```json
// Output message
{
    "payload": {
        "v": 79,
        "q": 0,
        "t": "2021-08-11T07:24:19.592Z"
    }
}
```

In the example above, the timestamp is what was assigned on the server side when the value was written.

The quality value `q` is not present in the return message If only `v` and `t` are present in the input message.
