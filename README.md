# WebStudio

WebStudio is a modern Web App where you can compile widgets on a grid.

| Item | Description |
| --- | --- |
| [Compilation Model](#Compilation) | WebStudio compilation model.
| [Actions](./actions/README.md) | Interaction actions.
| _Generic Widgets:_
| [Editor](./widgets/editor/README.md) | Display rich text editor.
| [Form](./widgets/form/README.md) | Display entries to collect user input.
| [IFrame](./widgets/iframe/README.md) | Embed other web content.
| [Image](./widgets/image/README.md) | Display an image.
| [Markdown Viewer](./widgets/markdownviewer/README.md) | Display Markdown content including Mermaid graphs.
| [Table](./widgets/table/README.md) | Display and edit tabular data. Supports (multi) select.
| [Tabs](./widgets/tabs/README.md) | Displaying multiple compilations within a main compilation.
| [Text](./widgets/text/README.md) | Display text.
| [Time Period Table](./widgets/timeperiodtable/README.md) | Table with start and end time pickers.
| [Tree](./widgets/tree/README.md) | Display hierarchical node structure.
| [Video](./widgets/video/README.md) | Display a video.
| _Specific Widgets:_
| [Batch Table](./widgets/batchtable/README.md) | Display batch (BPR) headers.
| [Chart](./widgets/chart/README.md) | Display trend chart with multiple axis support.
| [Event Table](./widgets/eventtable/README.md) | Display A&E events.
| [Faceplate](./widgets/faceplate/README.md) | Display actual system object values.
| [File Grid](./widgets/filegrid/README.md) | Display info and content of multiple files (GridFS).
| [Report Viewer](./widgets/reportviewer/README.md) | Display reports with export support.
| _Development Widgets:_
| [Message Debugger](./widgets/messagedebugger/README.md) | Inspecting action pipeline messages.
| [Transform Editor](./widgets/transformeditor/README.md) | Test transform actions.
| _Examples:_
| [Example Compilations](./compilations/README.md) | Examples of the widgets and their interaction.

## Compilation

The WebStudio configuration in which the applied widgets are defined is called a **compilation**.

## Model

The `version` is for future changes to be able to support new definition of the model.

```jsonc
{
    "version": "1",
    "widgets: [] // List of widget objects.
}
```

See [widget doc](./widgets/README.md) model description for the widget model.

### Options

```json
{
    "options": {
        "background": {},
        "numberOfColumns": 24,
        "numberOfRows": 12,
        "stacking": "none"
    }
}
```

| name | description |
| ---- | --- |
| `backgroundSize` | options are:
| | `auto`
| | `cover`
| | `contain`
| | `50%`
| | `12px`
| | `3.2em`
| | `50% auto`
| | `100px 100px`
| `numberOfColumns` | Defines how many vertical columns the grid is divided.
| `numberOfRows` | defines how many horizontal rows the grid is divided.
| `stacking` | Defines how the widgets can be positioned. Options are:
| | `none`: Widgets can be dragged around freely. **Default**
| | `vertical`: Widgets are stacked vertically.
| | `horizontal`: Widgets are stacked horizontally.
| `width`| To set the maximal grid width with e.g. 500.

Example to have an image as the compilation background:

```json
{
    "options": {
        
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

## Terminology

| name | description |
| ---- | ----------- |
| system | Refers to the system and/or the Web API.
| objspec | Object’s path or the object ID.

Read other parts of the docs to get familiar with the system capabilities.

## Loading WebStudio

WebStudio is shipped with the system Setup. The installation of the Web API also includes the WebStudio web app. WebStudio can be loaded into the Browser by:

`PROTOCOL`://`HOSTNAME`:`PORT`/apps/webstudio/

- `PROTOCOL`: http or https
- `HOSTNAME`: Hostname of the web server from where to load WebStudio from. Can be the Web API.
- `PORT`: TCP/IP port number of the web server.

Typically with a default installation you can open the Browser, on the machine where the Web API is installed, and visit:

```url
http://localhost:8002/apps/webstudio
```

By providing query parameters in the URL you can auto connect and load WebStudio compilations.

### Connection parameters

- `hostname=HOSTNAME`: Hostname of the Web API.
- `port=8002`: TCP/IP port number of the Web API.
- `ssl=true`: Using secure connection to the Web API.

### Security parameters

Adding credentials in the URL:

- `credentials=`: Base64 encoded 'username:password'.
- `authority=builtin`: Possible values `builtin`(default), `ad`, `machine`

Using Integrated Windows Authentication:

- `secp=iwa`:

### Loading options

- `logo=false`: removes the vendor logo.
- `theme=light`: Setting the theme to `dark` **(default)** or `light`.

### Loading WebStudio compilation from the system

If the compilation is loaded from a URL, WebStudio will automatically hide the development tools. In order to allow editing but still serve the compilation from the system the property `showDevTools` can be set to `true`.

```jsonc
{
    "options": {
        "showDevTools": true
    }
}
```

In case the user, typically an engineer, wants to edit a hosted compilation, the query parameter `dev=1` can be added to the URL. Keeps in mind that a `showDevTools` which is set to `false`, will overrule the query parameter and prevents the development tools to be available.

Development Tools are:

- Add widget
- Import compilation
- Export compilation
- Edit compilation model
- Edit widget model

#### Loading from custom Advanced Endpoint

Model can be loaded by calling an Advanced Endpoint. See Web API - Advanced Endpoint docs for more details.

- `lib=MY_LIBRARY`: Lua library name. Script can return function or a Lua table.
- `func=MY_FUNCTION`: Function name in case the library returns a Lua table.
- `farg=`: Function argument data. Typically Base64 encoded in case of JSON data.
- `ctx=/System/Core/MyFolder`: (Optional) Alternative script context.

Example:

```url
https://company.corp:8003/apps/webstudio/?hostname=company.corp&port=8002&ssl=true&secp=iwa&lib=my_librarys&func=my_function
```

#### Loading from object's Custom Property

WebStudio compilations can be loaded from Custom Properties of objects by adding two query parameter in the URL.

- `obj`: object path or id. (Note: if not provided the Web API default context path will be used.)
- `name`: name of the Custom Property.

Example:

```url
https://company.corp:8003/apps/webstudio/?hostname=company.corp&port=8002&ssl=true&secp=iwa&obj=/System/Core/MyFolder&name=display01
```
