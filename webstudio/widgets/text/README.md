# Text

This widget can be used to show some text, like an instruction on screen.

The style object accepts a wide range of css styles. The css properties are only accepted in camelCase. Some examples:

See valid text-color values: [Mozilla CSS: color](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value)  
See valid text-align values: [Mozilla CSS: text-align](https://developer.mozilla.org/en-US/docs/Web/CSS/text-align)  
See valid font-size values: [Mozilla CSS: font-size](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size)

## Model

```json
{
    "type": "text",
    "text": "Some instruction text",
    "options": {
        "style": {
            "backgroundColor": "#fefff4de",
            "color": "#00c302",
            "fontSize": "24px",
            "fontWeight": "bold",
            "fontFamily": "\"Courier New\", Courier, sans-serif",
            "textAlign": "left"
        }
    }
}
```

### Actions

Supported action hooks:

- `onClick`: when user clicks on the text label.

Example opening a link:

```json
{
    "type": "text",
    "text": "Click me",
    "options": {
        "style": {
            "color": "grey",
            "textAlign": "center",
            "fontSize": "26px",
            "fontWeight": "bold",
            "backgroundColor": "darkgreen"
        }
    },
    "actions": {
        "onClick": {
            "type": "openLink",
            "message": {
                "payload": {
                    "url": "https://www.lipsum.com"
                }
            }
        }
    }
}
```
