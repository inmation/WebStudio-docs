# Tabs

With this widget you can show different widgets on the same space by switching between tabs in the same browser tab. Each tab holds a complete 'sub' compilation.

## Model

```jsonc
{
    "type": "tabs",
    "tabs": [                   // List of tabs. View order depends on the index in the array.
        {
            "id": "",           // Unique id of the tab.
            "name": "",         // Name of the tab amd shown on the tab in case there is no title.
            "indicator": {
                "title": "",    // Show on the tab indicator.
                "icon": {},     // Icon config object.
                "style": {}     // Sets additional style for this indicator. (Different font or color)
            },
            "compilation": {}   // Like the a normal WebStudio compilation.
        }
    ]
}
```

Icon configuration.

```jsonc
{
    "icon": {
        "icon": "",             // Emoji, URL or Base64 for both dark and light theme.
        "dark": "",             // Use this field for icon in dark theme.
        "light": "",            // Use this field for icon in light theme.
    }
}
```

Icon image by an emoji.

```json
{
    "icon": {
        "icon": "📈"
    }
}
```

Icon image by a URL.

```json
{
    "icon": {
        "icon": {
            "url": ""
        }
    }
}
```

Icon image by a Base64 encoded string.

```json
{
    "icon": {
        "icon": {
            "base64": "",
            "mimeType": "image/png"
        }
    }
}
```

This is similar how the [Image widget](../image/README.md) is configured to load the image.

### Options

```jsonc
{
    "options": {
        "indicator": {          // Sets settings for all indicators that have not been configured on individual tab level.
            "style": {},        // Sets additional style for all indicators. (Different fonts or colors)
        },
        "tabAlignment": "bottom" | "left" | "right" | "top", // Sets the position of the tab bar according to the specified direction (default top).
        "showTabBar": true      // Set this to false will hide the tab bar. (default is true)
    }
}
```

### Actions

A tab can be dynamically changed by the [modify action](../../actions/README.md#modify). Based on the `route` the behavior is:

| route contains | results in |
| --- | --- |
| ID of the tabs |refresh of all the tabs |
| ID of the tabs and the tab | refresh of that particular tab |
| ID of the tabs and the tab and the widget | refresh of the widget on that particular tab |

This pattern for the `route` continues for nested `Tabs widget` within tabs.
