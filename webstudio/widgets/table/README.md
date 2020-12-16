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
        "path": "/System/Core/Examples/Tables/Example01"
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
    "dataSource": {
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
}
```

### Options

```json
{
    "options": {
        "editable": false,
        "multi": true,
        "multiMin": 1,
        "multiMax": 3,
        "style": {
            "backgroundColor": "grey",
            "color": "blue",
            "fontSize": "24px",
            "fontWeight": "bold",
            "fontFamily": "\"Courier New\", Courier, sans-serif",
            "textAlign": "left",
            "whiteSpace": "pre-line"
        },
        "pagination": true,
        "pageSize": 20,
        "alternateColumnColoring": true,
        "alternateRowColoring": true,
        "showHoverHighLight": true,
        "showSelectedRow": false,
        "allowSorting": false,
        "refreshButton": true,
        "refreshInterval": 30
    }
}
```

| name | description |
| ---- | --- |
| `pagination` | when set to false, disables the pagination functionality of the table, showing all the rows from the database on one page. Toolbar is also hidden if there is no `onSubmit` action defined.
| `showSelectedRow` | background color will change when a table row is clicked. Defaults to true when onSelect pipeline is defined.
| `showHoverHighLight` | enable/disable hover effect on the table rows.
| `allowSorting` | can disable sorting icons and sorting functionality.
| `showToolbar` | to hide the complete toolbar.
| `multi` | Allow selection of multi rows.
| `multiMin` | Minimal needed selected rows.
| `multiMax` | Maximum allow selected rows.
| `editable` | Editable cells.
| `resizeColumns` | Allow manually column resizing. (*)
| `refreshButton` | This will make sure a refresh button is visible and fetches the data from the system when pressed. By default the refresh button is visible.
| `refreshInterval` | Refresh with an interval in seconds.

`editable`:
> When editable is set to true the cell values can be modified. Note that this only affects the original dataset if the table is saved and the user has the  permissions to overwrite data.

`multiMax`:
> When `multi` is true, the `multiMin` and `multiMax` properties can limit how many table rows can be selected. If `multiMax` is set to `1`, the table renders radio buttons instead of checkboxes.

`resizeColumns`:
> When `resizeColumns` is set to `true` the columns are manually resizable with the resize icon at the right of the headers, or the desired column width can be added to the headers with the `width` property.

Refresh on a trigger. The widget subscribes to the system for data changes. This property is an objspec. (Not yet implemented) (*)

```json
{
    "refreshTrigger": "Object Path"
}
```

See valid fontSize values: [MDN web docs > CSS: Cascading Style Sheets > font-size](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size)

#### Styling

Conditional styling, which needs to be applied to all cells in a row, can be defined within `rules` in the `options` of the model. Cell / column based styling can be defined in within a `schema` element.

A row based rule needs to contain a `name` field to point to the right data field in the row. The `range` element can be used to define a numeric range by using the fields `from` and `to`. The value defined in the `to` field is excluded from the range. It is also possible to define only the `from` value to match all values greater than or equal to the value defined in the `from` field. To match all values less than a certain value, only the `to` field can be defined.

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

This widget supports the following topics to collect data:

- `selectedRows`: collects the selected rows data

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
| `editable` | Instead of making every cell editable with setting `editable: true` on the table, there's an option to add the editable property to certain columns only. Note that this only affects the original dataset if the table is saved and the user has the  permissions to overwrite data (optional, default is false)
| `width` | When resizeColumns is set to true, width can be defined for the headers manually (optional)
| `style` | Styling (optional)
| `hidden` | Property controlling column visibility (optional)

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
            "numberOfDecimals": 2
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
                    "name": "firstname"
                },
                {
                    "title": "Last Name",
                    "name": "lastname"
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
                    "label": "Montag",
                    "value": "Monday"
                },
                {
                    "label": "DiensTag",
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

### Actions

- `onClick`: Cell is clicked. Can be defined via the [Schema](###Schema).
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

The selected rows (array) are set on the input message `payload`. Whether rows can be selected with respect to the minimum and maximum number of selected rows, can be set with `multi`, `multiMin` and `multiMax` in the [Options](###Options).

```jsonc
{
    "actions": {
        "onSubmit": [] // Pipeline
    }
}
```
