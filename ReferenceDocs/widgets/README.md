# Widget

Common features of the WebStudio widgets.

Every widget needs to have a unique `id`. In case no `id` is set, WebStudio will generate one automatically. Feel free to adjust the `id` as long it is unique.

## Model

```jsonc
{
    "type": "widget type",
    "id": "yLtC", // Unique identification of the widget.
    "name": "NAME", // Will be shown in the caption bar.
    "description": "",
     "layout": {
        "w": 7,
        "h": 15,
        "x": 0,
        "y": 0,
        "locked": false
    }
}
```

In case `layout` is not provided, WebStudio will try to auto layout the widget.

### Caption bar

To hide the caption bar:

```json
{
    "captionBar": false
}
```

Advanced properties:

```jsonc
{
    "captionBar": {
        "hidden": true,
        "title": "Widget Title", // Will be used instead of the name field.
        "showModelEditorButton": false
    }
}
```

When `title` is not defined, the title will be "`name`" or `type` when `name` is not defined.
When `showModelEditorButton` is set to false, the editor of the widget becomes unavailable.

## Options

- `refreshInterval` lets the widget automatically refresh. Value is given in seconds. Minimum value is 0.5 seconds. The value 0 means that the refresh interval is disabled.

Every widget can be styled by setting the `style` object. The `style` object accepts a wide range of CSS styles. The CSS properties are only accepted in camelCase. Some examples:

```json
{
    "options": {
        "refreshInterval": 10,
        "style": {
            "backgroundColor": "transparent",
            "border": "1px solid blue"
        }
    }
}
```

Other `border` options are:

- `none`
- `1px dashed red`
- `1px dotted white`
- `4px double grey`
- `5px ridge white`
- `3px solid blue`

See more over here [CSS Borders](https://www.w3schools.com/css/css_border.asp).

## Action Pipeline

The widgets can perform logic in the form of actions. This can be a single action or a sequence of actions which we call an action pipeline. The data which is passed to and from one action to the other is called a message.

Actions can be defined within the [actions](#actions) field and in specific parts in the model. This is documented per widget type.

Performing multiple pipelines in parallel can be achieved by defining multiple arrays on the pipeline field.

### Message Flow

The input message of a pipeline action contains:

```jsonc
{
    "topic": "", // Could be provided by the widget.
    "payload": {} // Typically an object.
}
```

The `payload` can be any type. Keep in mind that in order to be able merged with the input message it needs to be an object. The `topic` tells the executing / receiving widget what to do with the `payload`.

In case the output of an action doesn't return an object with `topic` or a `payload` the output will be set to the input message `payload` of the next action.

With the next example only the payload is provided to the action.

```jsonc
{
    "type": "ACTION_TYPE"
}
```

In order to merge additional fields into the `payload` a pipeline action can be defined like:

```jsonc
{
    "type": "ACTION_TYPE",
    "message": {
        "payload": {
            "additionalValue": 42
        }
    }
}
```

In order to merge additional fields into the message a pipeline action can be defined like:

```jsonc
{
    "type": "ACTION_TYPE",
    "message": {
        "payload": {
            "additionalValue": 42
        },
        "additionalField": "Hello"
    }
}
```

### Skip an action step

If you want to skip an action step during testing you can set the `skip` field to `true`.

## DataSource

Data sources can be defined in the widget model to fetch and write data from and to the system. It can also be an action pipeline.

Obvious actions:

- `function`: Invokes an Advanced Endpoint.
- `read`: Reads the dynamic value or a property value of an object.
- `read-write`: Reads and writes the dynamic value or a property value of an object.
- `subscribe` : Subscribes to the `OnDataChanged` event of a dynamic value.
- `write`: Writes the dynamic value or a property value of an object.

Typically a `dataSource` has a single action.

```json
{
    "dataSource": {
        "type": "read",
        "path": "/System/Core/Examples/Variable"
    }
}
```

In order to `transform` the output message of the `read` action, an action pipeline can be defined.

```jsonc
{
    "dataSource": [
        {
            "type": "read",
            "path": "/System/Core/Examples/Variable"
        },
        {
            "type": "transform",
            "aggregateOne": [] // See transform docs.
        }
    ]
}
```

### Function

To invoke an Advanced Endpoint. The `payload` in the output contains the data returned by the Advanced Endpoint.

```jsonc
{
    "dataSource": {
        "type": "function",
        "lib": "LIBRARY NAME",
        "func": "FUNCTION NAME", // Optional function name in case library is Lua table.
        "farg": {}, // Optional function argument.
        "ctx": "" // Optional system object path.
    }
}
```

### Read

Reads the dynamic value of an object or the value of an object property. The `payload` in the output contains the dynamic or property value.

```json
{
    "dataSource": {
        "type": "read",
        "path": "/System/Core/Examples/Demo Data/Process Data/FC4711"
    }
}
```

### Read-Write (*)

Reads and writes the dynamic value of an object or the value of an object property.

```json
{
    "dataSource": {
        "type": "read-write",
        "path": "/System/Core/Examples/Demo Data/Process Data/FC4711"
    }
}
```

### Subscribe

Subscribes to the `OnDataChanged` event of a dynamic value. The `payload` in the output contains the dynamic value.

```json
{
    "dataSource": {
        "type": "subscribe",
        "path": "/System/Core/Examples/Demo Data/Process Data/FC4711"
    }
}
```

## Actions

The `actions` is an object which fields are known hooks, like the [Life Cycle Hooks](#Life-Cycle-Hooks), or custom action names. The value can be a single action object or an array with action objects.

```json
{
    "actions": {}
}
```

## Life Cycle Hooks

Widgets supports life cycle hooks. During loading, updating and refreshing many hooks are invoked. Hooks can contain any number of pipeline actions.

Overview of all the general hooks:

- `didLoad`: Widget loaded on screen.
- `willUpdate`: Widget will update view.
- `didUpdate`: Widget did update view.
- `willRefresh`: Widget will perform a refresh.
- `didRefresh`: Widget did perform a refresh.
- `willFetch`: Widget will perform a fetch.
- `didFetch`: Widget did perform a fetch.

The hooks`willFetch` and `didFetch` are only invoked if the widget has a data source defined. Note that some widgets have root properties, like `path` on a Faceplate widget, which are considered as a data source.

### Load Life Cycle Hooks

The load life cycle flow is:

1. `didLoad`
2. `willFetch`  
Execution of the data source action (pipeline)
3. `didFetch`
4. `willUpdate`  
Screen updated
5. `didUpdate`

### Refresh Life Cycle Hooks

The refresh life cycle flow is:

1. `willRefresh`
2. `willFetch`  
Execution of the data source action (pipeline)
3. `didFetch`
4. `willUpdate`  
Screen updated
5. `didUpdate`
6. `didRefresh`

Refresh can be executed by e.g. a refresh button, refresh interval and send action.

### Update Life Cycle Hooks

The update life cycle flow is:

1. `willUpdate`  
Screen updated
2. `didUpdate`
