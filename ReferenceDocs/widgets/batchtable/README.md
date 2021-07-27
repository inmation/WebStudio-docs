# Batch Table

This widget shows batch (BPR) headers in a tabular view. The widget derives from `Time Period Table` and contains Table and Time Period Pickers.

Note: Features marked with (*) are not supported yet.

## Model

```jsonc
{
    "type": "batchtable",
    "dataSource": {
        "type": "fetch",
        "path": "PATH TO BATCH OWNING OBJECT",
        "filter": {},
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

The widget will pass this object as message payload into the pipeline.

```json
{
    "batchid": "Batch_1601472379000",
    "duration": "00:01:50",
    "endtime": 1601472499000,
    "entryid": "c1c393ac-0320-11eb-80f1-e15ed41ae343",
    "starttime": 1601472389000
}
```
