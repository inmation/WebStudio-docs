# Event Table

This widget shows Alarms & Events in a tabular view. Contains Table and Time Period Pickers

Note: Features marked with (*) are not supported yet.

## Model

```jsonc
{
    "type": "eventtable",
    "dataSource": {
        "type": "fetch",
        "path": "PATH TO EVENT STREAM OBJECT",
        "filter": {}, // (optional) to filter the events.
        "project": {},
        "sort": {},
        "skip": 0,
        "limit": 1000
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
