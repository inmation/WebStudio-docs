# Image

This widget can be used to show one single image based on a URL or Base64 encoded image and mimeType. The `url` in the model is leading.

## Model using URL

```json
{
    "type": "image",
    "url": ""
}
```

## Model using Base64 encoded image

```json
{
    "type": "image",
    "mimeType": "image/png",
    "base64": ""
}
```

The following MIME types are considered safe for use on web pages and supported by modern web browsers:

| File format | MIME type | File extension(s) |
| ------------| ----------| ----------------- |
| Animated Portable Network Graphics | `image/apng` | .apng
| Bitmap file | `image/bmp` | .bmp
| Graphics Interchange Format | `image/gif` | .gif
| Microsoft Icon | i`mage/x-icon` |.ico, .cur
| Joint Photographic Expert Group image | `image/jpeg` | .jpg, .jpeg, .jfif, .pjpeg, .pjp
| Portable Network Graphics | `image/png` | .png
| Scalable Vector Graphics | `image/svg+xml` | .svg

### Options

```json
{
    "options": {
        "size": "contain"
    }
}
```

Possible values for `size`:

| value | description |
| ----- | ----------- |
| `contain` | Auto size with aspect ratio. **(default)**
| `100px` | Width 100 pixels and auto height with aspect ratio.
| `100px 50px` | Width of 100 pixels and height of 50 pixels.
| `100% 50%` | Width of 100 percentage and height of 50 percentage.
| `cover` | Fit with aspect ratio.

### Update Behavior

The update logic of the widget accepts a payload which is a string or a JSON object. In case the payload is a string the widget checks whether the string is a JSON string or base64 string. In case the string is base64 encoded, the `base64` field in the model will be updated. In case the payload is a JSON string or JSON object the values of the fields `url`, `base64` and `mimeType` will be used to change the fields in the model accordingly. In case the payload input is neither a base64 encoded or JSON string the value will be used to update the `url` in the model.

### Actions

Supported action hooks:

- `onClick`: when user clicks on the image.

Example opening a link:

```json
{
    "type": "image",
    "name": "Random Image",
    "url": "https://source.unsplash.com/random",
    "actions": {
        "onClick": {
            "type": "notify",
            "text": "Image clicked!"
        }
    }
}
```
