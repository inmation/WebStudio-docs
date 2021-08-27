# File Grid

This widget can be used to show files from the file stores (GridFS).

This widget was know as FileBrowser in the past.

## Model

```json
{
    "type": "filegrid",
    "name": "WorkCenter 01 files",
    "dataSource": {
        "type": "fetch",
        "limit": 1000,
        "path": "/Enterprise/Site/Area/WorkCenter01"
    },
    "storename": [
            "FILE_STORE_01"
    ]
}
```

### Options

pagination: When set to false, disables the pagination functionality of the filegrid, showing all the rows from the dataset on one page and it also hides the bottom toolbar.

```json
{
    "options": {
        "pagination": true,
        "refreshInterval": 30
    }
}
```

Lua

```lua
    local fileGrid = {
        id = "files",
        type = "filegrid",
        name = "WorkCenter 01 files",
        dataSource = {
            type = "fetch",
            limit = 1000,
            path = "/Enterprise/Site/Area/WorkCenter01"
        },
        storename = {
            "FILE_STORE_01"
        },
        options = {
            refreshInterval = 30
        },
        layout = {
            w = 12,
            h = 12,
            x = 0,
            y = 0,
            static = false
        }
    }
```
