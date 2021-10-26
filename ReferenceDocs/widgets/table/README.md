# Table

This widget shows a tabular view of data.

Note: Features marked with (*) are not supported yet.

## Model

```json
{
    "type": "table"
}
```

### Data from Table Holder / KPI Table

The widget fetches the table data from the system and does auto discovering of the header columns. This property is an objspec.

```json
{
    "dataSource": {
        "type": "read",
        "path": "/System/Core/Examples/WebStudio/Tables/SalesOrders"
    }
}
```

### Data from Advanced Endpoint

```json
{
    "dataSource": {
        "type": "function",
        "lib": "library-name",
        "func": "function-name",
        "farg": {
            "equipment": "MyEquipment",
            "threshold": 120
        }
    }
}
```

### Fixed Data

Fixed data set. Instead of the data coming from fetch, a fixed data set can be defined in the model.

```json
{
    "data": [
        {
            "name": "Inside Temperature",
            "value": 26
        }
        {
            "name": "Outside Temperature",
            "value": 18
        }
    ]
}
```

### Options

```json
{
    "options": {
        "allowSorting": false,
        "alternateColumnColoring": true,
        "alternateRowColoring": true,
        "editable": false,
        "multi": true,
        "multiMax": 3,
        "multiMin": 1,
        "pageSize": 20,
        "pagination": true,
        "showHoverHighLight": true,
        "showRefreshButton": true,
        "showSelectedRow": false,
        "showToolbar": true,
         "style": {
            "backgroundColor": "grey",
            "color": "blue",
            "fontFamily": "\"Courier New\", Courier, sans-serif",
            "fontWeight": "bold",
            "fontSize": "24px",
            "textAlign": "left",
            "whiteSpace": "pre-line"
        },
        "refreshInterval": 30
    }
}
```

| name | description |
| ---- | --- |
| `editable` | Editable cells.
| `allowSorting` | Can disable sorting icons and sorting functionality.
| `multi` | Allow selection of multi rows.
| `multiMin` | Minimal needed selected rows.
| `multiMax` | Maximum allow selected rows.
| `pagination` | When set to false, disables the pagination functionality of the table, showing all the rows from the database on one page. Toolbar is also hidden if there is no `onSubmit` action defined.
| `showHoverHighLight` | Enable/disable hover effect on the table rows.
| `showSelectedRow` | Background color will change when a table row is clicked. Defaults to true when onSelect pipeline is defined.
| `showRefreshButton` | Set this to `false` to hide the refresh button. By default the refresh button is visible.
| `showToolbar` | To hide the complete toolbar.
| `refreshInterval` | Refresh with an interval in seconds.

`editable`:
> When editable is set to true the cell values can be modified. Note that this only affects the original dataset if the table is saved and the user has the  permissions to overwrite data.

`multiMax`:
> When `multi` is true, the `multiMin` and `multiMax` properties can limit how many table rows can be selected. If `multiMax` is set to `1`, the table renders radio buttons instead of checkboxes.

```json
{
    "refreshTrigger": "Object Path"
}
```

See valid fontSize values: [MDN web docs > CSS: Cascading Style Sheets > font-size](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size)

#### Styling

Conditional styling, which needs to be applied to all cells in a row, can be defined within `rules` in the `options` of the model. Cell / column based styling can be defined in within a `schema` element.

A row based rule needs to contain a `name` field to point to the right data field in the row. The `range` element can be used to define a numeric or date range by using the fields `from` and `to`. The value defined in the `to` field is excluded from the range. It is also possible to define only the `from` value to match all values greater than or equal to the value defined in the `from` field. To match all values less than a certain value, only the `to` field can be defined.

The styles of all matching rules will be applied to all cells in the row.

```json
{
    "options": {
        "rules": [
            {
                "name": "temperature",
                "value": 20,
                "style": {
                    "backgroundColor": "green",
                    "fontSize": "15px",
                    "fontWeight": "normal"
                }
            },
            {
                "name": "temperature",
                "range": {
                    "from": 22,
                    "to": 26
                },
                "style": {
                    "fontWeight": "bold"
                }
            }
        ]
    }
}
```

### Receive messages (Send Topics)

This widget receive messages from others. Besides the generic, this widget also support topics:

- `selectRows` : Can be used to select one or more rows in the table. The message `payload` should contain a `rowIndex` array, which contains the index(es) of the rows which need to be selected.

Example to send a message to a table widget with an array of the selected rows

```jsonc
{
    "type": "send",
    "to": "table",
    "message": {
        "topic": "selectRows",
        "payload": {
            "rowIndex": [
                5,
                6
            ]
        }
    }
}
```

### Collect

Collect without a `topic` defined will result in the data rows.

This widget supports the following topics to collect data:

- No topic defined: collects all the data rows.
- `selectedRows`: collects the selected rows data.

Example of getting the selected rows data from a table widget by using an onClick action of a text Widget:

```jsonc
{
    "type": "text",
    "text": "Click here",
    "actions": {
        "onClick": [
            {
                "type": "collect",
                "from": "table",
                "message": {
                    "topic": "selectedRows"
                }
            },
            {
                "type": "consoleLog",
                "tag": "Selected Row Data"
            }
        ]
    }
}
```

### Schema

A schema in the model will overrule any returned schema from the data source.

A schema consist of:

| name | description
| ---- | ----------
| `title` | Column title.
| `name` | Key name of the row data.
| `filter` | Type of filter: `none`, `text`, `select`, `range`, `slider` (optional, default is `text`)
| `type` | Cell values can be displayed as different types. The format how the date and number type are displayed depend on the user's locale settings. Accepted types: `text`, `number`, `date` (optional, there is no default format)
| `format` | When type is `date`, the date format can be changed within the optional format property. Example of date formats: `"YYYY-MM-DD HH:mm:ss.SSS"`, `LLLL`, `DD.MM.YYYY HH:mm` (optional)
| `numberOfDecimals` | When type is `number`, the numberOfDecimals property can specify the number of digits shown after a decimal separator (optional)
| `items` | Cell value can be selected from a dropdown list containing predefined values.
| `editable` | Instead of making every cell editable with setting `editable: true` on the table, there's an option to add the editable property to certain columns only. Input type should match `type` declared in the cell. In case provided input does not match `type`or provided input is invalid, previous value will stay unchanged and notification will be raised to warn the user. Note that this only affects the original dataset if the table is saved and the user has the permissions to overwrite data (optional, default is false)
| `width` | Width can be defined for the columns manually by providing pixel values as string e.g. "300px". `Width` declared in the schema will overrule the default column size. (optional)
| `style` | Styling (optional)
| `hidden` | Property controlling column visibility (optional)
| `sort` | Property to define default sorting on the column. Accepted values: `asc` (ascending), `desc` (descending), `none` (no sorting). Default table sorting on multiple columns is possibly by defining `sort` in multiple schema items. The column order defines the sorting order. Note that sorting doesn't change the original row indexes (optional, default is none)
| `value` | Can be used to define the value for a fixed column. A fixed column can be created without pointing to a key in the table data. The `value` property is ignored when the schema items contains a `name` property.
| `isExpander` | This option can be used for [hierarchical data](#Hierarchical-Data) structures, it enables collapsing and expanding table rows. Set `isExpander: true` on a key of sub rows item, which is of type array. Key has to be defined in `data` structure. Global or column filtering of the table is not supported when this options is used. Default `collapsedIcon` and `expandedIcon` can be replaced by custom icons, e.g. emoji or special characters.

```json
{
    "schema": [
        {
            "name": "id",
            "hidden": true
        },
        {
            "title": "Population",
            "name": "population",
            "filter": "select",
            "type": "number",
            "editable": true
        },
        {
            "title": "Value",
            "name": "value",
            "filter": "slider",
            "type": "number",
            "numberOfDecimals": 2,
            "sort": "desc" 
        },
        {
            "title": "Fixed column",
            "value": "Click here",
            "actions": {
                "onClick": {
                    "type": "notify",
                    "text": "Fixed column cell clicked!"
                }
            }
        }
    ]
}
```

#### Grouped columns

```json
{
    "schema": [
        {
            "title": "Employee",
            "columns": [
                {
                    "title": "Name",
                    "name": "firstName"
                },
                {
                    "title": "Last Name",
                    "name": "lastName"
                },
                {
                    "title": "Age",
                    "name": "age",
                    "filter": "slider"
                }
            ]
        },
        {
            "title": "Address",
            "columns": [
                {
                    "title": "Street",
                    "name": "street",
                    "filter": "text"
                },
                {
                    "title": "City",
                    "name": "city"
                },
                {
                    "title": "Country",
                    "name": "country",
                    "filter": "select"
                }
            ]
        }
    ]
}
```

#### Dropdown selection

Allows to select a predefined input value from a dropdown. Predefined inputs need to be defined in the `items` array. This can be an array of strings or an array of objects containing a `label` and `value`. The latter can be used when the text to show should be different than the `value`.

Example `items` as string array:

```json
{
    "schema": [
        {
            "name": "day",
            "title": "Day of week",
            "items": [
                "Monday",
                "Tuesday",
                "Wednesday",
                "Thursday",
                "Saturday",
                "Sunday"
            ]
        }
    ]
}
```

Example `items` by defining labels and values:

```json
{
    "schema": [
        {
            "name": "day",
            "title": "Day of week",
            "items": [
                {
                    "label": "MON",
                    "value": "Monday"
                },
                {
                    "label": "TUE",
                    "value": "Tuesday"
                }
            ]
        }
    ]
}
```

#### Formatting

 ```jsonc
 {
     "schema": [
        {
            "name": "temperature",
            "title": "Temperature",
            "type": "number",
            "numberOfDecimals": 2,
            "engUnit": "°C"
        },
        {
            "name": "timestamp",
            "title": "Timestamp",
            "type": "date",
            "format": "YYYY-MM-DD HH:mm:ss.SSS"
        },
        {
            "name": "status",
            "title": "Status",
            "enumMode": "valueToName", // valueToName (default), nameToValue
            "enum": {
                "Dry": 1,
                "Wet": 2
            }
        }
     ]
 }
 ```

Enum formatting can do `valueToName` (default) or `nameToValue`.

- `valueToName`: Table cell value will be matched with a value of an enum and the name will be shown in the table cell.
- `nameToValue`: Table cell value will be matched with a name of an enum and the value will be shown in the table cell.

#### Style

Cell or column based styling can be defined in the `style` within a `schema` element.

```json
 {
    "schema": [
        {
            "name": "value",
            "title": "Value",
            "style": {
                "backgroundColor": "#00c302",
                "fontSize": "15px",
                "fontWeight": "bold",
                "whiteSpace": "pre-line"
            }
        }
     ]
 }
 ```

Conditional styling on schema level applies for to cell. The `name` field is optional and can be defined in case the value match needs to be done with another data field. The styles of all matching rules will be applied to the cell.

Cell based rules:

```json
 {
    "schema": [
        {
            "name": "value",
            "title": "Value",
            "style": {
                "backgroundColor": "#00c302",
                "fontSize": "15px",
                "fontWeight": "bold",
                "whiteSpace": "pre-line"
            },
            "rules": [
                {
                    "value": 1,
                    "style": {
                        "backgroundColor": "green",
                        "fontSize": "15px",
                        "fontWeight": "normal"
                    }
                },
                {
                    "value": 2,
                    "style": {
                        "color": "yellow"
                    }
                },
                {
                    "range": {
                        "from": 5,
                        "to": 10
                    },
                    "style": {
                        "color": "yellow"
                    }
                }
            ]
        }
     ]
 }
```

#### Hierarchical Data

Data field has a key `children`, which is used in schema with `isExpander` to create hierarchical table layout.

```json
    "data": [
        {
            "Item": "Pencil",
            "Total": 189.05,
            "Unit Cost": 1.99,
            "children": [
                {
                    "Item": "Pencil",
                    "Total": 59.7,
                    "Unit Cost": 1.99,
                    "OrderDate": "2019-06-08"
                },
                {
                    "Item": "Pencil",
                    "Total": 59.7,
                    "Unit Cost": 1.99,
                    "OrderDate": "2019-06-10"
                }
            ]
        }
    ]
```

```json
    "schema": [
        {
            "name": "children",
            "collapsedIcon": "▷",
            "expandedIcon": "▽",
            "isExpander": true
        },
        {
            "name": "Item",
            "title": "Item"
        },
        {
            "name": "Total",
            "title": "Total"
        },
        {
            "name": "Unit Cost",
            "title": "Unit Cost"
        },
        {
            "name": "OrderDate",
            "title": "Order date",
            "type": "date",
            "format": "YYYY-MM-DD"
        }
    ],
```

### State
The table widget maintains some dynamic state information in the `state` property to record the currently selected row(s) as well as the active search and filter options. 

```jsonc
{
    "state": {
        // list of 0 based row indexes of selected rows 
        "selectedRowIndex": [], 
        // list of filter object
        "filters": [
            {
                "name": "Count",
                "value": [
                    2,
                    5
                ]
            }
        ],
        // Search text
        "search": {
            "value": ""
        }
    }
}
```
These may be set in the initial model, but it is more common to access and manipulate the values at run-time. 

**Note**: The state properties can be modified using action pipelines to explicitly set the filters, selected rows or search values. When applying such changes the current state fields need to be overwritten in their entirety. Changing fields inside the root level properties is not supported. 

To put this into perspective, consider the following example: 
Suppose we want to change the **"range"** filter on the **"Count"** field so it ends at 10 rather then 5, as in the example above. 

We might be tempted to so something like this:

```jsonc
{
    "type": "modify",
    "id": "table",
    "set": [
        {   // This is not supported !
            "name": "model.table.state.filters.0.value.1",
            "value": 10
        }
    ]
}
```

What we need to do instead, is to provide the complete filter array. 

```jsonc
{
    "type": "modify",
    "id": "table",
    "set": [
        {
            // The model field being modified is the 
            // "filter" array
            "name": "model.table.state.filters",
            // The value assigned to it must includes 
            // all filter settings to be applied.
            "value": [
                {
                    "value": [
                        3,
                        10
                    ],
                    "name": "Count"
                }
            ]
        }
    ]
} 
```

In this case only one field is involved in the filter, but if there were more, they would all need to be included in the `filters` array even if only one of them changes.

### Actions

- `onClick`: Cell is clicked. Can be defined via the [Schema](#schema).
- `onSelect`: Single row is selected.
- `onSelectionChanged`: Perform an action each time the selection is changed.
- `onSubmit`: Submit button is clicked and selected rows are in the message payload.

#### onClick

Cell click can be defined to a whole table column. This by setting the `onClick` action in a `schema` item.

```jsonc
{
    "schema": [
        {
            "name": "value",
            "title": "Value",
            "actions": {
                "onClick": [] // Can be a single action or action pipeline.
            }
        }
    ]
}
```

The input message `payload` will contain the complete row data and the selected cell info. Example:

```json
{
    "row": {
        "_idx": 1,
        "name": "Temperature",
        "value": 26
    },
    "cell": {
        "_idx": 2,
        "name": "value",
        "value": 26
    }
}
```

#### onSelect

Gets invoked when a row is selected. The input message `payload` contains the selected row data.

```jsonc
{
    "actions": {
        "onSelect": [
            {
                "type": "function",
                "lib": "library-name",
                "func": "function-name",
                "farg": {} // This will be set automatically.
            }
        ]
    }
}
```

Transforming the selected row data and invoke a function:

```jsonc
{
    "actions": {
        "onSelect": [
            {
                "type": "transform",
                "aggregateOne": [] // This will return one object instead of an array.
            },
            {
                "type": "function",
                "lib": "library-name",
                "func": "function-name",
                "farg": {} // This will be set automatically.
            }
        ]
    }
}
```

Send a message to another widget with selected row data

```jsonc
{
    "actions": {
        "onSelect": [
            {
                "type": "send",
                "to": "widget01"
            }
        ]
    }
}
```

#### onSelectionChanged

Gets invoked when a row is (de)selected. The input message `payload` contains all the selected rows (array).

```jsonc
{
    "actions": {
        "onSelectionChanged": [] // Can be a single action or action pipeline.
    }
}
```

#### onSubmit

The selected rows (array) are set on the input message `payload`. Whether rows can be selected with respect to the minimum and maximum number of selected rows, can be set with `multi`, `multiMin` and `multiMax` in the [Options](#Options).

```jsonc
{
    "actions": {
        "onSubmit": [] // Pipeline
    }
}
```
