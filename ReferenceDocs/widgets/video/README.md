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

Any valid video attributes can be added to the model. Visit '[The Video Embed element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video)' to see the full list of attributes.

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
