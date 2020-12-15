# WebStudio

Note: Features marked with (*) are not supported yet.

## Model

The `version` is for future changes to be able to support new definition of the model.

```jsonc
{
    "version": "1",
    "widgets: [] // List of widget objects.
}
```

### Options

```json
{
    "options": {
        "stacking": "none",
        "numberOfColumns": 24,
        "numberOfRows": 12,
        "background": {
            "style": {
                "backgroundColor": "#001b01",
                "backgroundImage": "url(\"https://transparenttextures.com/patterns/45-degree-fabric-light.png\")",
                "backgroundSize": "contain"
            }
        }
    }
}
```

`stacking` defines how the widgets can be positioned. Options are:

- `none`: widgets can be dragged around freely. **Default**
- `vertical`: widgets are stacked vertically.
- `horizontal`: widgets are stacked horizontally.

`numberOfColumns` defines how many vertical columns the grid is divided.
`numberOfRows` defines how many horizontal rows the grid is divided.

`backgroundSize` options are:

- `auto`
- `cover`
- `contain`
- `50%`
- `12px`
- `3.2em`
- `50% auto`
- `100px 100px`

`width` to set the maximal grid width with e.g. 500.

### Widgets

A list of widgets. See [widget doc](./widgets/README.md) model description for the widget model.

```json
{
    "widgets": [
    ]
}
```

## Loading WebStudio

`PROTOCOL`://`HOSTNAME`:`PORT`/apps/webstudio/

- `PROTOCOL`: http or https
- `HOSTNAME`: Hostname of the web server from where to load WebStudio from. Can be the Web API.
- `PORT`: TCP/IP port number of the web server.

Query parameters:

- `hostname=HOSTNAME`: Hostname of the Web API.
- `port=8002`: TCP/IP port number of the Web API.
- `ssl=true`: Using secure connection to the Web API.

### Adding security in the query parameters

Adding credentials in the URL:

- `credentials=`: Base64 encoded 'username:password'.
- `authority=builtin`: Possible values `builtin`(default), `ad`, `machine`

Using Integrated Windows Authentication:

- `secp=iwa`:

## Loading WebStudio compilation from custom Advanced Endpoint

Model can be loaded by calling an Advanced Endpoint. See Web API - Advanced Endpoint docs for more details.

- `lib=MY_LIBRARY`: Lua library name. Script can return function or a Lua table.
- `func=MY_FUNCTION`: Function name in case the library returns a Lua table.
- `farg=`: Function argument data. Typically Base64 encoded in case of JSON data.
- `ctx=/System/Core/MyFolder`: (Optional) Alternative script context.

Example:

```url
https://company.corp:8003/apps/webstudio/?hostname=company.corp&port=8002&ssl=true&secp=iwa&lib=my_librarys&func=my_function
```

## Loading WebStudio compilation from object's Custom Property

WebStudio compilations can be loaded from Custom Properties of objects by adding two query parameter in the URL.

- `obj`: object path or id. (Note: if not provided the Web API default context path will be used.)
- `name`: name of the Custom Property.

Example:

```url
https://company.corp:8003/apps/webstudio/?hostname=company.corp&port=8002&ssl=true&secp=iwa&obj=/System/Core/MyFolder&name=display01
```
