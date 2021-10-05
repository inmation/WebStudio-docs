# Tabs

With this widget you can show different widgets in the same space by switching between tabs in the same browser tab. Each tab holds a complete 'sub' compilation.

## Model

```jsonc
{
    "type": "tabs",
    "options" : {},             // Set tabs level options. see details below        
    "tabs": [                   // List of tabs. View order depends on the index in the array.
        {
            "id": "",           // Unique id of the tab.
            "name": "",         // Name of the tab as shown on the tab when there is no title.
            "indicator": {},    // Configure the appearance of the tab's indicator. see details below
            "compilation": {}   // Like the a normal WebStudio compilation.
        }
    ]
}
```
More details for the nested properties are provided below:
### Options
The options property controls the overall appearance of the tabs indicator:

```jsonc
{
    "options": {
        "tabAlignment": "top",  // Edge to show the tab indicator at
        "showTabBar": true,     // Show or hide the tab indicator bar
        "indicator": {}         // General indicator settings 
    }
}
```

| Name | Description |
| --- | --- |
| `tabAlignment`| Show the tab indicators at any of the widgets edges ("top" = default, "bottom", "left", "right" ). |
| `showTabBar` | The tab-bar can be explicitly hidden. This is useful to maximize the available screen area available to the contained compilations. If the tab bar is hidden, different tab instances can still be selected using [send actions](#receive-messages) as explained below |
| `indicator` | The indicator settings provide a means to configure the appearance of the tab selector in the tab bar. Overall settings can be defined at the `tabs` level or instance specific settings can be supplied per `tab`.<br>**Note**: The `indicator` setting at `tabs` level does not support `title` or `icon` properties, since these relate to individual tab instances.|

### Indicator
The appearance of the indicator, used to show which tab is selected, can be configured at the `tabs` level, in which case it applies to all `tab` instances, or at the individual `tab` level.

```jsonc
{
    "indicator": {
        "title": "",    // Title shown on the tab indicator.
        "icon": {},     // Icon config object.
        "style": {},    // Sets additional style for this indicator. (e.g. Different font or color)       
        "selector": {   // Settings relating to the selected tab
            "line": {
                "color": "red",
                "alignment": "bottom",
                "hidden": false
            },
            "style": {
                "color": "yellow"
            }
        }
    }
}
```
| Name | Description |
| --- | --- |
| `title` | Title of the tab in the indicator bar. Only relevant at `tab` level. If provided overrides the `name` field|
| `icon` | Only relevant at `tab` level. Used to display an optional [icon](#icon-configuration) for the tab.  |
| `style` | Set the "base" appearance of indicators independent of their selected state. Setting defines in the `selector` section will override these.|
| `selector` | Configure the appearance of the selected tab's indicator. When defined at the `tabs` level the settings are applied to any selected `tab` instances. The `selector` has the following sub properties:<br>- `line`: Configure the color and alignment ("top", "bottom", "left", "right") of a line drawn on an edge of the selected tab. For tab indicators at the top and bottom of the widget, the default line position will be on the inside edge of the tab canvas. When the indicator bar is at the left or right edges, the line defaults to the outside edges of the widget. <br>- `hidden`: The line can be omitted using the `hidden` property. **Note**: If the line is hidden, the selected tab will be shown with a different background color to allow it to be easily identified. The default color can be overridden by setting the `style`<br>- `style`: Provide style settings specific to the selected tab, such as font and background color settings. |

### Icon configuration
Tab specific icons can be provided in addition to, or instead of indicator titles. 
> Set the `indicator` title to an empty string ("") if only the icon should be displayed
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

This is similar to how the [Image widget](../image/README.md) is configured.
### data sources
As with other widgets, model content can be loaded from a data source. The `tabs` widget is somewhat unique in this regard, since it provides `dataSource` properties at both the `tabs` level and at individual `tab` level. 

Typically the data source will be an advanced endpoint used to call a Lua function in the backend. The script needs to return a Lua table conforming to the model expected by the widget. As always, the returned Lua table is transparently translated to json.  

* **Tabs dataSource**: The number of tabs returned and their appearance will be under the control of the `dataSource`. The data returned must be one of the following:
  * **Tabs array**: An array of objects each element of which defines a `tab` object as shown above. The compilation content of the tab can be included in the returned dataset, but may be left blank {} if the `tab` in question has its own `dataSource` binding. If the model returned contains data sources at tab level, these are evaluated immediately after the top level update has been done.
  * **Tabs object**: A single object, containing a field called "tabs" which is an array of `tab` objects as described in the previous point.
* **Tab dataSource**: The `tab` level `dataSource` can return one of the following:
  * **Compilation object**: A single object containing a valid compilation. WebStudio looks for the presence of `version` and `widgets` properties to determine if the returned object is a compilation. If either of these is not present, WebStudio will treat the object as a `tab` object
  * **Tab object**: An object with named fields of a `tab`. Any of the fields can be provided  except for the `id` which cannot be overwritten by the data source at `tab` level. **Note**: The `tabs` data source can define any field of a `tab`

### Actions
A tab can be dynamically changed by means of a [modify action](../../actions/README.md#modify). Based on the `route` the behavior is:

| route contains | results in |
| --- | --- |
| ID of the tabs |refresh of all the tabs |
| ID of the tabs and the tab | refresh of that particular tab |
| ID of the tabs, the tab and the widget | refresh of the widget on that particular tab |

This pattern for the `route` continues for nested `Tabs widget` within tabs.

#### Examples:
* Change the tab indicator alignment:
```jsonc
{
    "type": "modify",
    "id": { // even though we are not modifying anything on the tab instances,
            // they are all refreshed by modifying a property at tabs level! 
        "route": [ "tabs01" ]
    },
    "set": [
        { 
            "name": "model.options.tabAlignment",
            "value": "left"
        }
    ]
}
``` 

* Change the indicator color of "tab01". Only the targeted tab will be updated.
```jsonc
{
    "type": "modify",
    "id": { // only tab01 will be refreshed.
        "route": [ "tabs01", "tab01" ]
    },
    "set": [
        { 
            "name": "model.indicator.style.color",
            "value": "yellow"
        }
    ]
}
``` 

* To change the property of a widget inside the compilation of a tab, say the color of a `text` widget, you might be tempted to do something like this:

```jsonc
{
    "type": "modify",
    "id": {
        "route": [
            "tabs01",
            "tab01"
        ]
    },
    "set": [
        { // Access a widget by its array index in the compilation.
          // This is not very convenient and can lead to unpredictable 
          // results if the position of the widget were to change in the array.
            "name": "model.compilation.widgets.0.options.style.color",
            "value": "yellow"
        }
    ]
}
```
A much better way of achieving the same is by specifying the id of the target widget in the `route` expression, which automatically interprets any id following the tab id as that of a widget in its compilation. Not only does it make the expression simpler and more robust, but it also ensured that only the targeted widget is updated instead of the whole tab compilation.

```jsonc
{
    "type": "modify",
    "id": {
        "route": [
            "tabs01",
            "tab01",
            "text01"
        ]
    },
    "set": [
        {
            "name": "model.options.style.color",
            "value": "yellow"
        }
    ]
}
```

#### Receive messages
The `tabs` widget responds to the following message topics in a [send action](../../actions/README.md#send), which can be used to dynamically activate a specific tab.

* `setActiveTab`: Requires some additional parameters to be supplied in the message payload:

  | Field | Description |
  | --- | --- |
  | `activate` | Optional field used to controls which tab should be selected. Options are:<br>-  Relative to the currently selected tab or the provided `id` : "next", "previous", "nextWithRotate", "previousWithRotate". The last two options cause the selection to wrap round if the referenced tab at the last or first position. <br>- Absolute: "first" or "last" |
  | `id` | Causes the tab with the specified `id` to be activated when used without providing an `activate` value. When used in conjunction with `activate`, determines the tab from which the relative position is calculated.  

Examples: 

Move to the next tab in the tabs array:
```jsonc
{
    "type": "send",
    "to": "tabs01",
    "message": {
        "topic": "setActiveTab",
        "payload": {
            "activate": "next" // move relative to the current active tab 
        }
    }
}
```

Active a specific tab:
```json
{
    "type": "send",
    "to": "tabs01",
    "message": {
        "topic": "setActiveTab",
        "payload": {
            "id": "tab02" // activate is not provided
        }
    }
}
```

> **Note**: The selected tab can be found by inspecting the widget's work model. It will contain a `state` field  
