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

Property `modelRoot` declares from which KPI item to start the Model tree view. If this property is not set Model will display all alowed KPI model items.
By setting property `includeRoot` to `true`, Model's tree view will be displayed including item declared in the `path`.
By setting property `includeRoot` to `false`, Model's tree view will be displayed starting from the item's declared in the `path` children.

```jsonc
{
    "type": "chart",
    "name": "My Chart",
    "modelRoot": {
                "path": "/example Charts",
                "includeRoot": false
            },
    "chart": {} // Chart model
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

- `addCursors` : add one or multiple cursors
- `addPens` : add one or multiple pens
- `setCursors` : update the set of cursors
- `setTagTable` : set the tag table by object path
- `setTimeSpan` : set the timespan on x-axis

#### addCursors

Send topic `addCursors` adds one or multiple cursors. In case the cursor already exists the cursor in the payload will be ignored. Send `timestamp` in the payload to add the cursor on that timestamp. `Timestamp` can be send in ISO or Epoch format. 

```json
"onClick": [
    {
        "type": "send",
        "to": "chartwidgetID",
        "message": {
            "topic": "addCursors",
            "payload": [
                {
                    "timestamp": 1613481658185
                },
                {
                    "timestamp": 1613482658185
                }
            ]
        }
    }
]
```

To `clear cursors` send empty payload.

```json
"onClick": [
    {
        "type": "send",
        "to": "chartwidgetID",
        "message": {
            "topic": "setCursors",
            "payload": []
        }
    }
]
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

Send topic `setCursors` will fully update the set of cursors in the chart. Send `timestamp` in the payload to set the cursor on that timestamp. `Timestamp` can be send in ISO or Epoch format. 

```json
"onClick": [
    {
        "type": "send",
        "to": "chartwidgetID",
        "message": {
            "topic": "setCursors",
            "payload": [
                {
                    "timestamp": 1613480658185
                }
            ]
        }
    }
]
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
                        "to": "webchart1",
                        "message": {
                            "topic": "addPens"
                        }
                    }
                ]
            }
        },
        {
            "type": "webchart",
            "id": "webchart1",
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
