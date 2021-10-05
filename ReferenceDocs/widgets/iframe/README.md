# IFrame

This widget can be used to show images or videos.

## Model using URL

```json
{
    "type": "iframe",
    "url": ""
}
```

## IFrame Options

Any valid iframe attributes can be added to the model. See the full list of attributes here: [The Inline Frame element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe)

```json
{
    "iframeOptions": {
        "allowFullScreen": true
     }
}
```
## Receive messages (Send Topics)
The IFrame's content can be reloaded by [`sending`](../../actions/README.md#send) it a message with the topic set to `reload`. This options is however being deprecated, since it has been superseded by the more generic `update` topic which is supported by all widgets.

```json
{
    "type": "send",
    "to": "iframe",
    "message": {
        "topic": "update"
    }
}
```
