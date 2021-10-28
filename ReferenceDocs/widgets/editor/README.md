# Editor

The widget can be used to show any kind of text like JSON or scripting code.

## Model

```jsonc
{
    "type": "editor",
    "content": {}, // Optional content to be shown. Can be a string or JSON.
    "contentToCompare" : {}, // optional
    "language" : "json"
}
```
| Name | Description |
| --- | --- |
| `content` | Primary editor content. This can be either text or JSON. |
| `contentToCompare` | Setting this property activates the editor's compare functionality which shows the difference between the `content` and the `contentToCompare` side by side.|
| `language` | The language setting is used to activate the editor's syntax coloring and checking. <br>**Supported languages:**<br><ul><li>`json` **Default**<li> `lua`<li> `markdown`<li> `txt`<li> `xml`

### Options

```json
{
    "options": {
        "showToolbar": false
}
```

### Editor Options

See [Editor Options](https://microsoft.github.io/monaco-editor/api/interfaces/monaco.editor.ieditoroptions.html) for all the available editor options.

```json
{
    "editorOptions": {}
}
```

When the editor is used to compare content, either of the panels can be configured to be editable or read-only by setting the `readOnly` and `originalEditable` options as follows:

| Editable panels | `readOnly` | `originalEditable` |
| --- | --- | --- |
| Both panels | false | true |
| Neither panel | true | false | 
| Only the `content` | true | true |
| Only the `contentToCompare` | false | false |

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
