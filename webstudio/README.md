# WebStudio Compilation

The WebStudio configuration in which the applied widgets are defined is called a **compilation** and sometimes referred to as the **grid model**.

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
