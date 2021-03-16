# Plotly

This widget can be used to generate graphs. It is build based on plotly.js, which is a high-level declarative charting library. Plotly.js ships with over 40 chart types.

View Plotly documentation [here](https://plotly.com/javascript/).

This widget is data driven, data structure should conform to the Plotly documentation provided at the beginning of each different type of chart's section.

## Configuration Options

* Scroll and Zoom
* Editable
* Static Plot
* Display ModeBar - either always or never display mode bar
* Display Logo - disable or enable Plotly logo on mode bar

```json
"plotlyOptions": {
    "config": {
        "scrollZoom": true,
        "editable": true,
        "staticPlot": true,
        "displayModeBar": true,
        "displaylogo": true
    }
}
```

## General Layout Options

* Paper Background Color
* Plot Background
* Show Legend
* Title (font)
* X-axis (title, tickangle, gridwidth)
* Y-axis (title, tickangle, gridwidth)

### Example

```json
"plotlyOptions": {
    "layout": {
        "paper_bgcolor": "steelblue",
        "plot_bgcolor": "steelblue",
        "showlegend": false,
        "title": "Click Here<br>to Edit Chart Title",
        "font":{
            "family": "Raleway, sans-serif"
            },
        "xaxis": {
            "tickangle": -45,
            "gridwidth": 2,
            "title": {
                "text" : "normalized moisture"
                },
            },
        "yaxis": {
            "tickangle": -45,
            "gridwidth": 2,
            "title": {
                "text": "normalized pressure"
            }
        }
    }
}
```

## Data

### Data from Data Holder Item

The widget fetches the data from the system by `read` or `subscribe`.

Example of fetching data in the widget:

```json
{
    "dataSource": {
        "type": "read",
        "path": "/System/Core/Examples/PlotlyData"
    }
}
```

Example of data model saved as JSON string in the system:

```json
{
    "data": [
        {
            "type": "scatterpolar",
            "r": [
                19,
                28,
                14
            ],
            "theta": [
                "A",
                "B",
                "C"
            ],
            "fill": "toself",
            "name": "Group A"
        }
    ]
}
```

### Data from Advanced Endpoint

```jsonc
{
    "type": "function",
    "lib": "LIBRARY NAME",
    "func": "FUNCTION NAME", // Optional function name in case library is Lua table.
    "farg": {}, // Optional function argument.
    "ctx": "" // Optional system object path.
}
```

## onClick events

Plotly [Click Events](https://plotly.com/javascript/plotlyjs-events/#click-event) documentation.

Plotly chart can handle on click triggered event. Based on a click event on a Plotly chart, the `onClick` action defined in the Widget model is executed.

When `onClick` action is defined, received data format depends on Plotly `Display ModeBar` mode selection. Default mode is `Compare data on hover`, with this selection all data points that are defined on the same x axis will be received. Another option is `Show closest data on hover`, when this mode is selected only selected data point is received.

Charts that do not support click events:

* Gauge charts
* Bullet chart
* Indicator

```jsonc
"actions": {
        "onClick": [
            {
                "type": "send",
                "to": "debugger"
            },
            {
                "type": "transform",
                "aggregateOne": [
                    {
                        "$project": {
                            "text": "$points.0.label"
                        }
                    }
                ]
            }
```

## Scatter Plots

Plotly [Scatter Plots](https://plotly.com/javascript/line-and-scatter/) documentation.

* Mode (Markers, markers+text, line+markers)
* Name
* Marker (size, color, opacity)
* Text (labels, text position, text font)

```json
"data": [
    {
        "type": "scatter",
        "x": [
            1,
            2
        ],
        "y": [
            10,
            15
        ],
        "mode": "markers+text",
        "name": "Team A",
        "text": [
            "A-1",
            "A-2"
        ],
        "textposition": "top center",
        "textfont": {
            "family": "Raleway, sans-serif"
        },
        "marker": {
            "size": 12
        }
    }
]
```

## Line Charts

Plotly [Line Charts](https://plotly.com/javascript/line-charts/) documentation.

* Line (lines, markers, lines+markers)
* Name
* Marker (size, color, opacity)
* Line (color, opacity, width)

```json
"data": [
    {
        "type": "scatter",
        "x": [
            1,
            2
        ],
        "y": [
            10,
            15
        ],
        "mode": "lines+markers",
        "marker": {
            "color": "red",
            "size": 8
        },
        "line": {
            "color": "red",
            "width": 1
        }
    }
]
```

## Bar Charts

Plotly [Bar Charts](https://plotly.com/javascript/bar-charts/) documentation.

* Orientation (h, v)
* Name
* Text
* Text Position
* Hover Info
* Marker (color, opacity, line(color, opacity, width))

```json
"data": [
    {
        "type": "bar",
        "x": [
            "Line 1",
            "Line 2"
        ],
        "y": [
            20,
            14
        ],
        "name": "Cologne",
        "text": [
            "20",
            "14"
        ],
        "textposition": "auto",
        "hoverinfo": "none",
        "marker": {
            "color": "blue",
            "opacity": 0.6,
            "line": {
                "color": "blue",
                "width": 1.5
            }
        }
    }
]
```

Layout Options:

* Bar Mode (group, stack)
* Bar Gap

```json
"plotlyOptions": {
    "layout": {
        "barmode": "group",
        "bargap" :0.05
    }
}
```

## Pie Charts

Plotly [Pie Charts](https://plotly.com/javascript/pie-charts/) documentation.

* Labels
* Domain (row and column or x and y)
* Hole (for donut chart)
* Text Info (label, percent, label+percent)
* Inside Text Orientation (radial)
* Auto margin
* Text Position (outside (inside is default))

```json
"data": [
    {
        "values": [
            10,
            30,
            60
        ],
        "labels": [
            "1st",
            "2nd",
            "3rd"
        ],
        "type": "pie",
        "textinfo": "label+percent",
        "textposition": "outside",
        "insidetextorientation": "radial",
        "automargin": true,
        "hole": 0.4,
        "domain": {
            "row": 0,
            "column": 0
        }
    }
]
```

Layout Options:

* Height
* Width
* Grid (rows, columns)

```json
"plotlyOptions": {
    "layout": {
        "height": 400,
        "width": 500,
        "grid": {
            "rows": 2,
            "columns": 2
        }
    }
}
```

## Filled Area Plots

Plotly [Filled Area Plots](https://plotly.com/javascript/filled-area-plots/) documentation.

* Fill (tozeroy, tonexty, toself)
* Fill Color
* Mode (none)
* Stack Group
* Group Norm (percent)
* Hoveron (points+fills)
* Line (color, opacity)
* Text
* Hoverinfo (text)

```json
"data": [
    {
        "type": "scatter",
        "x": [
            1,
            2
        ],
        "y": [
            0,
            2
        ],
        "fill": "tonexty",
        "mode": "none"
    }
]
```

Layout Options:

* X-axis (range)
* Y-axis (range)

```json
"plotlyOptions": {
    "layout": {
        "xaxis": {
            "range": [
                0,
                5
            ]
        },
        "yaxis": {
            "range": [
                0,
                3
            ]
        }
    }
}
```

## Box Plot

Plotly [Box Plot](https://plotly.com/javascript/box-plots/) documentation.

* Box Points
* Jitter
* Point Pos
* Whisker Width
* Fill Color
* Marker (color, opacity)
* Line
* Box points (false, outliers, suspectedoutliers),
* Box mean (true, sd)
* Orientation (h, v)

```json
"data": [
    {
        "type": "box",
        "y": [
            0,
            1,
            1,
            2,
            3,
            5,
            8,
            13,
            21
        ],
        "boxpoints": "all",
        "jitter": 0.3,
        "pointpos": 0,
        "whiskerwidth": 0.2,
        "fillcolor": "white",
        "boxpoints": "Outliers",
        "marker": {
            "size": 5
        },
        "line": {
            "width": 1
        }
    }
]
```

Layout Options:

* Box Mode (group, stack, overlay)

```json
"plotlyOptions": {
    "layout": {
        "boxmode": "group"
    }
}
```

## Histogram

Plotly [Histograms](https://plotly.com/javascript/histograms/) documentation.

* Name
* Marker (color, opacity line(color, opacity, width))
* Auto Bin x
* Auto Bin y

```json
"data": [
    {
        "type": "histogram",
        "y": [
            20,
            21,
            23,
            21,
            22,
            28,
            29,
            30,
            19,
            33
        ],
        "marker": {
            "color": "blue",
            "opacity": 0.2
        }
    },
    {
        "type": "histogram",
        "y": [
            25,
            26,
            28,
            20,
            21,
            23,
            21,
            22,
            28,
            29
        ],
        "marker": {
            "color": "red",
            "opacity": 0.2
        }
    }
]
```

Layout Options:

* Bar Mode (group, stack, overlay)
* Bar Gap
* Bar Group Gap

```json
"plotlyOptions": {
        "layout": {
            "paper_bgcolor": "white",
            "barmode": "overlay",
            "bargap": 0.05,
            "bargroupgap": 0.2
        }
    }
```
