# Chart

The chart is an advanced chart widget. The ability to display multiple axes (both x and y) means Web Chart is a great way to illustrate a lot of information within limited space and allow new insight to the data you may have otherwise missed.

Multiple x-axis support enables different time period comparisons. Diverse production data can be added to a single graph, making the comparison of critical outliers much easier by removing the need to switch between graphs. Multiple y-axis support enables different scale comparison. Variables with different scales can have a strong relation, e.g. temperature and humidity. Thanks to multi y-axis support these relations can now be easily illustrated. Combination of both multi x and y-axis makes Web Chart a powerful tool for chart plotting, analysis and trend exploration.

Web Chart also offers a wider range of chart types, such as Candlestick, Spline, Waterfall and others for robust data visualization.

## Model

```jsonc
{
    "type": "chart",
    "name": "My Chart",
    "chart": {} // Chart model
}
```

### Model Root

Property `modelRoot` declares from which KPI Model object to start showing in the model tree view. If this property is not set the model will display the whole KPI Model object in respect to the object permissions.
By setting property `includeRoot` to `true`, the model view will display the specified object including its children.
By setting property `includeRoot` to `false`, the model view will display the children of specified object.

```jsonc
{
    "modelRoot": {
        "path": "/Company/Plant A", // Object path of an object in the KPI Model.
        "includeRoot": false // Default is false.
    }
}
```

### Chart Model

```jsonc
{
    "class": "Trend",
    "name": "My Trend",
    "x_axis": [],           // Array of X Axis objects
    "y_axis": [],           // Array of Y Axis objects
    "pens": []              // Array of Pen objects
}
```

### X Axis Model

X Axis object properties.

```jsonc
{
    "id": 1,
    "name": "X-1",
    "description": "Simulated Flow 1",
    "start_time": "*-1d",
    "end_time": "*",
    "intervals_no": 100,
    "position": {
        "alignment": "bottom",
        "end": 100,
        "orientation": "bottom",
        "start": 0,
        "value": 1
    },
    "themes": {
        "dark": {
            "color": "white"
        },
        "light": {
            "color": "black"
        }
    },
    "locked": true,
    "grid": false,
    "ticks": {
        "count": 2
    }
}
```

| name | description |
| ---- | --- |
| `cursors` | (optional) Array of cursor objects.
| `end_time` | End time of the axis.
| `grid` | Enable/disable grid lines. If ticks are defined the grid lines are aligned with the ticks. (boolean)
| `id` | Axis id.
| `intervals_no` | Number of intervals.
| `locked` | Lock/unlock axis (boolean)
| `name` | Name of the axis.
| `position`| Object holding configuration properties related to axis positioning (see below)
| `start_time` | Start time of the axis.
| `themes` | Object holding styling options (see below)
| `ticks` | (optional) Object with tick configuration options (see below)

Position:

| name | description |
| ---- | --- |
| `alignment` | `bottom` / `top`
| `end` | Margin end
| `orientation` | `bottom` / `top`
| `start` | Margin start
| `value` | Position value (number between 1 and 8)

Ticks:

| name | description |
| ---- | --- |
| `count` | Number of ticks between start en end tick. Omit this to have it on auto.

### Tag Table

When a `tagtable` is provided a tabular icon is shown in the inspector at the pens section. By this the user can add additional pens to the chart.

```jsonc
{
    "tagtable": [
        {
            "Name": "",         // Pen name
            "Path": "",         // Path to the object
            "Description": ""   // (Optional)
        }
    ]
}
```

### Y Axis Model

```jsonc
{
    "id": 1,
    "name": "Y",
    "position": {
        "alignment": "left",
        "end": 100,
        "orientation": "left",
        "start": 0,
        "value": 1
    },
    "range": {
        "max": {
            "mode": "auto",
            "value": 0
        },
        "min": {
            "mode": "auto",
            "value": 0
        }
    },
    "themes": {
        "dark": {
            "color": "white"
        },
        "light": {
            "color": "black"
        }
    },
    "locked": false,
    "grid": false,
     "ticks": {
        "count": 2
    }
}
```

### Pens

```jsonc
{
    "id": 1,
    "name": "Simulated Density",
    "path": "/System/Core/Examples/Demo Data/Process Data/DC4711",
    "DecimalPlaces": 2,             // Overrules the object DecimalPlaces property
    "OpcEngUnit": "$",              // Overrules the object OpcEngUnit property
    "trend_type": "HT_LINE",
    "aggregate": "AGG_TYPE_INTERPOLATIVE",
    "style": {
        "line": "SOLID",
        "marker": "MARKER_STYLE_NONE",
        "thickness": "SEMISMALL"
    },
    "themes": {
        "dark": {
            "color": "aqua"
        },
        "light": {
            "color": "aqua"
        }
    },    
    "x_axis": [
        1
    ],
    "y_axis": [
        1
    ],
    "draw": true
}
```

### Options

```json
{
    "options": {
        "leftPanel": false,
        "rightPanel": false,
        "bottomPanel": false,
        "play": "live"
    }
}
```

| name | description |
| ---- | --- |
| `leftPanel` | hide or show panel
| `rightPanel` | hide or show panel
| `bottomPanel` | hide or show panel
| `play` | `live` or `refresh`

### Receive messages (Send Topics)

This widget receive messages from others. Besides the generic, this widget also support topics:

- [`addCursors`](#addcursors) : add one or multiple cursors
- [`addPens`](#addpens) : add one or multiple pens
- [`setCursors`](#setcursors) : update the set of cursors
- [`setTagTable`](#settagtable) : set the tag table by object path
- [`setTimeSpan`](#settimespan) : set the time span on x-axis

#### addCursors

Send topic `addCursors` adds one or multiple cursors. In case the cursor already exists the cursor in the payload will be ignored. Send `timestamp` in the payload to add the cursor on that timestamp. The `timestamp` can be send in ISO, Epoch, or relative time format.
If next to the timestamp, a `context` field is defined in the object, the cursor will show the value on hover. The `context` can be defined as a string, object, or array.

```json
{
    "type": "send",
    "to": "chartWidgetID",
    "message": {
        "topic": "addCursors",
        "payload": [
            {
                "timestamp": 1613481658185
            },
            {
                "timestamp": "*-1h",
                "context": "Show this on cursor hover"
            }
        ]
    }
}
```

To `clear cursors` send empty payload.

```json
{
    "type": "send",
    "to": "chartWidgetID",
    "message": {
        "topic": "setCursors",
        "payload": []
    }
}
```

#### addPens

Add pen(s) with the provided list of pen objects or object paths. The `addPens` topic type provides a way to add pen(s) to a chart. (This can be particularly useful when firing an onSelect action - e.g on table row select)

Add pen(s) by providing the object paths:

```json
{
    "type": "send",
    "to": "ChartWidget",
    "message": {
        "topic": "addPens",
        "payload": {
            "pens": [
                "/System/Core/Examples/Demo Data/Process Data/DC4711",
                "/System/Core/Examples/Demo Data/Process Data/DC666",
                "/System/Core/Examples/Demo Data/Process Data/FC4711"
            ]
        }
    }
}
```

Add pen(s) by providing pen objects:

```json
{
    "type": "send",
    "to": "ChartWidget",
    "message": {
        "topic": "addPens",
        "payload": {
            "pens": [
                {
                    "name": "FC4711",
                    "path": "/System/Core/Examples/Demo Data/Process Data/FC4711",
                    "style": {
                        "line": "SOLID",
                        "marker": "MARKER_STYLE_NONE",
                        "thickness": "SEMISMALL"
                    },
                    "trend_type": "HT_LINE"
                },
                {
                    "name": "DC666",
                    "path": "/System/Core/Examples/Demo Data/Process Data/DC666",
                    "style": {
                        "line": "SOLID",
                        "marker": "MARKER_STYLE_NONE",
                        "thickness": "SEMISMALL"
                    },
                    "trend_type": "HT_LINE"
                }
            ]
        }
    }
}
```

#### setCursors

Send topic `setCursors` will fully update the set of cursors in the chart. Send `timestamp` in the payload to set the cursor on that timestamp. The `timestamp` can be send in ISO, Epoch, or relative time format. The `timestamp` can be send in ISO, Epoch, or relative time format.
If next to the timestamp, a `context` field is defined in the object, the cursor will show the value on hover. The `context` can be defined as a string, object, or array.

```json
{
    "type": "send",
    "to": "chartWidgetID",
    "message": {
        "topic": "setCursors",
        "payload": [
            {
                "timestamp": 1613480658185
            },
            {
                "timestamp": "*-1h",
                "context": "Show this on cursor hover"
            }
        ]
    }
}
```

#### setTagTable

Setting the tag table by object path and prompt the Tags dialog.

```json
{
    "type": "send",
    "to": "ChartWidget",
    "message": {
        "topic": "setTagTable",
        "payload": {
            "tagTable": "/System/Core/Examples/WebStudio/Tables/Process Data Tags",
            "prompt": true
        }
    }
}
```

#### setTimeSpan

If a time period in the form of `starttime` and/or `endtime` is received, the widget will change the X-Axis accordingly when those X-Axis is not locked.

Changing the time period.

```json
{
    "type": "send",
    "to": "ChartWidget",
    "message": {
        "topic": "setTimeSpan",
        "payload": {
            "starttime": "2020-10-01T14:00:00.000Z",
            "endtime": "2020-10-01T16:00:00.000Z"
        }
    }
}
```

## Examples

Complete chart model:

```json
{
    "type": "chart",
    "name": "My Chart",
    "chart": {
        "class": "Trend",
        "description": "Demo Data/Process Data",
        "name": "Demo Process Data - FC4711",
        "pens": [
            {
                "draw": true,
                "id": 1,
                "name": "Simulated Flow on X-1",
                "path": "/System/Core/Examples/Demo Data/Process Data/FC4711",
                "aggregate": "AGG_TYPE_INTERPOLATIVE",
                "style": {
                    "line": "SOLID",
                    "marker": "MARKER_STYLE_NONE",
                    "thickness": "SEMISMALL"
                },
                "themes": {
                    "dark": {
                        "color": "aqua"
                    },
                    "light": {
                        "color": "aqua"
                    }
                },
                "trend_type": "HT_LINE",
                "x_axis": [
                    1
                ],
                "y_axis": [
                    1
                ]
            },
            {
                "draw": true,
                "id": 1,
                "name": "Simulated Flow on X-2",
                "path": "/System/Core/Examples/Demo Data/Process Data/FC4711",
                "style": {
                    "line": "SOLID",
                    "marker": "MARKER_STYLE_NONE",
                    "thickness": "SEMISMALL"
                },
                "themes": {
                    "dark": {
                        "color": "green"
                    },
                    "light": {
                        "color": "green"
                    }
                },
                "trend_type": "HT_LINE",
                "x_axis": [
                    2
                ],
                "y_axis": [
                    1
                ]
            }
        ],
        "x_axis": [
            {
                "description": "Simulated Flow 1",
                "end_time": "*",
                "grid": false,
                "id": 1,
                "intervals_no": 100,
                "locked": true,
                "name": "X-1",
                "position": {
                    "alignment": "bottom",
                    "end": 100,
                    "orientation": "bottom",
                    "start": 0,
                    "value": 1
                },
                "start_time": "*-1d",
                "themes": {
                    "dark": {
                        "color": "white"
                    },
                    "light": {
                        "color": "black"
                    }
                }
            },
            {
                "description": "Simulated Flow 2",
                "end_time": "*",
                "grid": false,
                "id": 2,
                "intervals_no": 100,
                "locked": false,
                "name": "X-2",
                "position": {
                    "alignment": "bottom",
                    "end": 100,
                    "orientation": "bottom",
                    "start": 0,
                    "value": 2
                },
                "start_time": "*-1d",
                "themes": {
                    "dark": {
                        "color": "white"
                    },
                    "light": {
                        "color": "black"
                    }
                }
            }
        ],
        "y_axis": [
            {
                "description": "m³/h",
                "grid": false,
                "id": 1,
                "locked": false,
                "name": "Y",
                "position": {
                    "alignment": "left",
                    "end": 100,
                    "orientation": "left",
                    "start": 0,
                    "value": 1
                },
                "range": {
                    "max": {
                        "mode": "fixed",
                        "value": 100
                    },
                    "min": {
                        "mode": "fixed",
                        "value": 0
                    }
                },
                "themes": {
                    "dark": {
                        "color": "white"
                    },
                    "light": {
                        "color": "black"
                    }
                }
            }
        ]
    }
}
```

Example to add a pen to the trend by selecting a row n a table:

```json
{
    "version": "1",
    "widgets": [
        {
            "type": "table",
            "id": "table2",
            "options": {
                "multi": false
            },
            "data": [
                {
                    "name": "DC4711",
                    "path": "/System/Core/Examples/Demo Data/Process Data/DC4711"
                },
                {
                    "name": "DC666",
                    "path": "/System/Core/Examples/Demo Data/Process Data/DC666"
                },
                {
                    "name": "FC4711",
                    "path": "/System/Core/Examples/Demo Data/Process Data/FC4711"
                },
                {
                    "name": "FC666",
                    "path": "/System/Core/Examples/Demo Data/Process Data/FC666"
                },
                {
                    "name": "TC4711",
                    "path": "/System/Core/Examples/Demo Data/Process Data/TC4711"
                },
                {
                    "name": "TC666",
                    "path": "/System/Core/Examples/Demo Data/Process Data/TC666"
                },
                {
                    "name": "PC4711",
                    "path": "/System/Core/Examples/Demo Data/Process Data/PC4711"
                },
                {
                    "name": "PC666",
                    "path": "/System/Core/Examples/Demo Data/Process Data/PC666"
                },
                {
                    "name": "QC4711",
                    "path": "/System/Core/Examples/Demo Data/Process Data/QC4711"
                },
                {
                    "name": "QC666",
                    "path": "/System/Core/Examples/Demo Data/Process Data/QC666"
                }
            ],
            "layout": {
                "w": 7,
                "h": 12,
                "x": 0,
                "y": 0,
                "static": false
            },
            "actions": {
                "onSelect": [
                    {
                        "type": "transform",
                        "aggregateOne": [
                            {
                                "$group": {
                                    "pens": {
                                        "$push": "$path"
                                    }
                                }
                            }
                        ]
                    },
                    {
                        "type": "send",
                        "to": "editor"
                    },
                    {
                        "type": "send",
                        "to": "chart1",
                        "message": {
                            "topic": "addPens"
                        }
                    }
                ]
            }
        },
        {
            "type": "chart",
            "id": "chart1",
            "layout": {
                "w": 17,
                "h": 9,
                "x": 7,
                "y": 0,
                "static": false
            }
        },
        {
            "type": "editor",
            "id": "editor",
            "name": "Debug message",
            "layout": {
                "w": 17,
                "h": 3,
                "x": 7,
                "y": 9,
                "static": false
            }
        }
    ]
}
```

## Lua

An example to define a chart widget in Lua.

```lua
local chart01 = {
    layout = {
        w = 12,
        h = 6,
        x = 0,
        y = 6,
        locked = false
    },
    id = "chart01",
    type = "chart",
    name = "Chart 01",
    chart = {
        class = "Trend",
        description = "Demo Data/Process Data",
        name = "Demo Process Data - FC4711",
        x_axis = {
            {
                end_time = "*",
                grid = false,
                id = 1,
                intervals_no = 100,
                locked = true,
                name = "X-1",
                position = {
                    alignment = "bottom",
                    ["end"] = 100,
                    orientation = "bottom",
                    start = 0,
                    value = 1
                },
                start_time = "*-1d",
                themes = {
                    dark = {
                        color = "white"
                    },
                    light = {
                        color = "black"
                    }
                }
            },
            {
                end_time = "*",
                grid = false,
                id = 2,
                intervals_no = 100,
                locked = false,
                name = "X-2",
                position = {
                    alignment = "bottom",
                    ["end"] = 100,
                    orientation = "bottom",
                    start = 0,
                    value = 2
                },
                start_time = "*-1d",
                themes = {
                    dark = {
                        color = "white"
                    },
                    light = {
                        color = "black"
                    }
                }
            }
        },
        y_axis = {
            {
                id = 1,
                grid = false,
                locked = false,
                name = "Y",
                position = {
                    alignment = "left",
                    ["end"] = 100,
                    orientation = "left",
                    start = 0,
                    value = 1
                },
                range = {
                    max = {
                        mode = "fixed",
                        value = 100
                    },
                    min = {
                        mode = "fixed",
                        value = 0
                    }
                },
                themes = {
                    dark = {
                        color = "white"
                    },
                    light = {
                        color = "black"
                    }
                }
            }
        },
        pens = {
            {
                id = 1,
                name = "Simulated Flow on X-1",
                path = "/System/Core/Examples/Demo Data/Process Data/FC4711",
                aggregate = "AGG_TYPE_INTERPOLATIVE",
                draw = true,
                style = {
                    line = "SOLID",
                    marker = "MARKER_STYLE_NONE",
                    thickness = "SEMISMALL"
                },
                themes = {
                    dark = {
                        color = "aqua"
                    },
                    light = {
                        color = "aqua"
                    }
                },
                trend_type = "HT_LINE",
                x_axis = {
                    1
                },
                y_axis = {
                    1
                }
            },
            {
                id = 1,
                name = "Simulated Flow on X-2",
                path = "/System/Core/Examples/Demo Data/Process Data/FC4711",
                aggregate = "AGG_TYPE_INTERPOLATIVE",
                draw = true,
                style = {
                    line = "SOLID",
                    marker = "MARKER_STYLE_NONE",
                    thickness = "SEMISMALL"
                },
                themes = {
                    dark = {
                        color = "green"
                    },
                    light = {
                        color = "green"
                    }
                },
                trend_type = "HT_LINE",
                x_axis = {
                    2
                },
                y_axis = {
                    1
                }
            }
        }
    }
}
```
