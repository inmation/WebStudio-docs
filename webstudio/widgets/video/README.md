# Video

The widget can be used to show videos.

## Model

```json
{
    "type": "video",
    "url": "",
    "mimeType": "video/mp4"
}
```

### Video Options

Any valid video attributes can be added to the model. See the full list of attributes here: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video.

Please note that the attributes have to be written in lowerCamelCase in order for the option to take effect. For example "autoPlay" instead of "autoplay".

```json
{
    "videoOptions": {
        "autoPlay": true,
        "muted": true,
        "loop": false,
        "controls": true
    }
}
```
