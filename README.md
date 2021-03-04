# WebStudio

WebStudio is a modern Web App where you can compile widgets on a grid.

| Item | Description |
| --- | --- |
| [Compilation Model](./webstudio/README.md) | WebStudio compilation model.
| _Generic Widgets:_
| [Editor](./webstudio/widgets/editor/README.md) | Display rich text editor.
| [Form](./webstudio/widgets/form/README.md) | Display entries to collect user input.
| [IFrame](./webstudio/widgets/iframe/README.md) | Embed other web content.
| [Image](./webstudio/widgets/image/README.md) | Display an image.
| [Markdown Viewer](./webstudio/widgets/markdownviewer/README.md) | Display Markdown content including Mermaid graphs.
| [Table](./webstudio/widgets/table/README.md) | Display and edit tabular data. Supports (multi) select.
| [Text](./webstudio/widgets/text/README.md) | Display text.
| [Time Period Table](./webstudio/widgets/timeperiodtable/README.md) | Table with start and end time pickers.
| [Video](./webstudio/widgets/video/README.md) | Display a video.
| _Specific Widgets:_
| [Batch Table](./webstudio/widgets/batchtable/README.md) | Display batch (BPR) headers.
| [Chart](./webstudio/widgets/chart/README.md) | Display trend chart with multiple axis support.
| [Event Table](./webstudio/widgets/eventtable/README.md) | Display A&E events.
| [Faceplate](./webstudio/widgets/faceplate/README.md) | Display actual system object values.
| [File Grid](./webstudio/widgets/filegrid/README.md) | Display info and content of multiple files (GridFS).
| [Report Viewer](./webstudio/widgets/reportviewer/README.md) | Display reports with export support.
| _Development Widgets:_
| [Message Debugger](./webstudio/widgets/messagedebugger/README.md) | Inspecting action pipeline messages.
| [Transform Editor](./webstudio/widgets/transformeditor/README.md) | Test transform actions.

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

- `theme=light`: Setting the theme to `dark` **(default)** or `light`.

### Loading WebStudio compilation from custom Advanced Endpoint

Model can be loaded by calling an Advanced Endpoint. See Web API - Advanced Endpoint docs for more details.

- `lib=MY_LIBRARY`: Lua library name. Script can return function or a Lua table.
- `func=MY_FUNCTION`: Function name in case the library returns a Lua table.
- `farg=`: Function argument data. Typically Base64 encoded in case of JSON data.
- `ctx=/System/Core/MyFolder`: (Optional) Alternative script context.

Example:

```url
https://company.corp:8003/apps/webstudio/?hostname=company.corp&port=8002&ssl=true&secp=iwa&lib=my_librarys&func=my_function
```

### Loading WebStudio compilation from object's Custom Property

WebStudio compilations can be loaded from Custom Properties of objects by adding two query parameter in the URL.

- `obj`: object path or id. (Note: if not provided the Web API default context path will be used.)
- `name`: name of the Custom Property.

Example:

```url
https://company.corp:8003/apps/webstudio/?hostname=company.corp&port=8002&ssl=true&secp=iwa&obj=/System/Core/MyFolder&name=display01
```
