# Time Period Table

Contains Table and Time Period Pickers

## Model

```json
{
    "type": "timeperiodtable",
    "dataSource": {
        "type": "function",
        "lib": "LibName"
    }
}
```

### Optional

Initial `starttime` and `endtime` can be given to initialize the Time Pickers.

```json
{
    "starttime": "2020-10-01T00:00:00.000Z",
    "endtime": "2020-10-01T00:00:00.000Z"
}
```

### Containment

The widget contains other widget(s) and can be accessed directly by:

```jsonc
{
    "table": {} // Table Widget.
}
```

See [Table](../table/README.md) widget for details.

### Actions

Additional hooks:

- `onSelect`: Invoked when row is selected.

#### onSelect action

```json
{
    "table": {
        "actions": {
            "onSelect": []
        }
    }
}
```
