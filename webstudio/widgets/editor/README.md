# Editor

The widget can be used to show any kind of text like JSON or scripting code.

## Model

```jsonc
{
    "type": "editor",
    "content": {}, // Optional content to be shown. Can be a string or JSON.
    "language" : "json"
}
```

Supported languages:

- `json` **Default**
- `lua`
- `markdown`
- `txt`
- `xml`

In case you want to see the whole input message set `completeMsgObject` to `true`.

```json
{
    "completeMsgObject": true
}
```

### Options

```json
{
    "options": {
        "showToolbar": false
}
```

### Editor Options

See [Editor Options](https://microsoft.github.io/monaco-editor/api/interfaces/monaco.editor.ieditoroptions.html)

```json
{
    "editorOptions": {}
}
```

### Actions

Supported actions:

- `collect`
- `send`
- `onContentChange`

### onContentChange

In case `onContentChange` is defined this will be invoked every time the textual content of the widget is updated. Can be used together with the `send` action pipeline to send the content to an other widget.

```json
{
    "onContentChange": [
        {
            "type": "send",
            "to": "Place here the ID of the widget"
        }
    ]
}
```

## Update Behavior

The update logic of this widget accepts a payload, which is a string or a JSON element. In case the value of the `payload` is a string, the value will be used to update the `content` in the model. When the `language` in the model model is set to `json`, the widget tries to convert the string into a JSON element. In case the JSON conversion fails, the initial `payload` string value will be used to update the `content` in the model.

When the `payload` is a JSON object, the widget uses the fields `content` and `language` to update the fields in the model accordingly. In case the `payload` does not contain the `content` field, the JSON object in the `payload`  will be used to update the `content` in the model. When the `content` field in the payload is a string and the `language` field in the model or payload is set to `json`, the widget tries to convert the value of the `content` in the `payload` to a JSON element.
