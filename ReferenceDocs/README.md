# WebStudio Reference

This part of the documentation is primarily aimed at providing technical reference material and as such isn't designed to be read end to end. While this may well be a good exercise to undertake in terms of getting to know all the tool has to offer, it is likely to be a rather dry read... For a more user friendly walk-through of how to use WebStudio and examples of compilations, refer to the "[Getting Started](../GettingStarted/README.md)" section instead.

Refer to the tables below to jump to specific reference section. 

| Item | Description |
|---|---|
| [Loading WebStudio](#loading-webstudio) | Load WebStudio in a browser
| [Compilation Model](#compilation) | WebStudio compilation model. |
| [Actions](actions/README.md) | Interaction actions. |

| Generic Widgets: | |
|---|---|
| [Editor](widgets/editor/README.md) | Display rich text editor. |
| [Form](widgets/form/README.md) | Display entries to collect user input. |
| [IFrame](widgets/iframe/README.md) | Embed other web content. |
| [Image](widgets/image/README.md) | Display an image. |
| [Markdown Viewer](widgets/markdownviewer/README.md) | Display Markdown content including Mermaid graphs. |
| [Table](widgets/table/README.md) | Display and edit tabular data. Supports (multi) select. |
| [Tabs](widgets/tabs/README.md) | Displaying multiple compilations within a main compilation. |
| [Text](widgets/text/README.md) | Display text. |
| [Time Period Table](widgets/timeperiodtable/README.md) | Table with start and end time pickers. |
| [Tree](widgets/tree/README.md) | Display hierarchical node structure. |
| [Video](widgets/video/README.md) | Display a video. |

| Specific Widgets: | |
|---|---|
| [Batch Table](widgets/batchtable/README.md) | Display batch (BPR) headers. |
| [Chart](widgets/batchtable/README.md) | Display trend chart with multiple axis support. |
| [Event Table](widgets/eventtable/README.md) | Display A&E events. |
| [Faceplate](widgets/faceplate/README.md) | Display actual system object values. |
| [File Grid](widgets/filegrid/README.md) | Display info and content of multiple files (GridFS). |
| [Report Viewer](widgets/reportviewer/README.md) | Display reports with export support. |

| Development Widgets: | |
|---|---|
| [Message Debugger](widgets/messagedebugger/README.md) | Inspecting action pipeline messages. |
| [Transform Editor](widgets/transformeditor/README.md) | Test transform actions. |

| Examples: | |
|---|---|
| [Example Compilations](../GettingStarted/compilations/README.md) |Examples of the widgets and their interaction. |

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

If the compilation is loaded from a URL, WebStudio automatically hides the development tools. If the user, typically an engineer, wants to edit a hosted compilation, the query parameter `dev=1` can be added to the URL. Also refer to the `showDevTools` option below which can be set directly on the compilation to control the visibility of the toolbar icons.

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

## Compilation

The WebStudio configuration, in which widgets along with their display attributes and dynamic behavior are defined, is collectively referred to as a **compilation**.

The compilation and contained widgets within are described using JSON objects. Throughout this documentation the term **"model"** refers to the structure of the various JSON fragments used to describe the elements within the compilation as well as the compilation as a whole.

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
        "numberOfColumns": 96,
        "numberOfRows": 12,
        "stacking": "none", 
        "showDevTools" : true,
        "padding": {
            "x": 0,
            "y": 2
        },
        "spacing": {
            "x": 2,
            "y": 2
        }
    }
}
```

| name | description |
| ---- | --- |
| `background` | Used to set the compilation background color and image. These settings are actually nested within a style object. Refer to the example below to see them in action |
|&nbsp;&nbsp;`style` | style options for the background |
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`backgroundImage` | load an image as the canvas backdrop. |
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`backgroundSize` | Set the display options for the `backgroundImage`. The options derived from CSS styles and are: |
| |`auto` : The loaded image native size is used and it is tiled to fill the whole canvas area. This is the **default** options |
| |`cover` : The image height is stretched to that of the canvas. If it has a wider aspect ratio than that of the browser window, the image will be clipped. Also note the if the image height is larger than that of the canvas, it will be clipped at the bottom of the frame|
| | `contain`: The image is repeated either horizontally or vertically, depending on which of its dimensions divides the most into the canvas width or height. In other words, if the background image is about the same width as the work area, but much shorter in height, it is repeated vertically and vise versa. |
| |`<n>%`: This option behaves the same as `auto` except that the image is scaled by the specified percentage (For example `50%` ) |
| | `<n>px` or `<n>px <m>px`: Sets the size of the image in pixels. If only one value is provided it is applied to the width of the image. For example `100px` is equivalent to `100px auto`  |
| | `<n>.<m>em`: Specify the size in EM units. `3.2em` for example |
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`backgroundColor`| can be set to any valid CSS color name or RGB value |
| `numberOfColumns` | Defines the number of vertical columns the grid is divided into. The absolute column width in pixels, will scale proportionally to that of the compilation. By default the `numberOfColumns` is set to 96 when you create a new compilation. <br>In case you are wondering why 96 and not 100, it is because 96 has the most factors, allowing the columns to be divided into more equal width groups ( 2, 3, 4, 6 ... etc) that 100. That said, the column count should be adjusted to suite the layout needs of your compilation.|
| `numberOfRows` | The naming of this property is a bit misleading in that it is used to set the height of grid rows to a specific number of pixels rather than dividing the canvas into equally sized rows. By setting this property, the row height is fixed and no longer scales when the canvas width is resized. If the property is omitted each cell in the grid is as many pixels high as it is wide. The width being determined by the canvas width divided by the `numberOfColumns`. In this case the row height does scales with the column width.
| `stacking` | Defines how the widgets can be positioned. Options are:
| | `none`: Widgets can be dragged around freely and positioned on any gid cell. **Default**
| | `vertical`: Widgets are stacked vertically starting from the top of the screen.
| | `horizontal`: Widgets are stacked horizontally starting at the left of the screen.
| `showDevTools` | As the name suggests, this parameter is used to control the availability of developer tools in the UI. It would be set to `true` for example to allow editing of compilations served from the system, or `false` to prohibit editing at any time.<br>**Note**: this setting takes precedence over the `dev` URL query parameter described above. In other words, if `showDevTools` is set to `false`, this will override the query parameter and prevents the development tools from being shown.|
| `padding`| This is an optional property used to set the number of pixels between the edge of the canvas and the content. The `x` value sets the padding at the left and right edges of the canvas, while the `y` value sets the padding at the top. Since compilations can be taller that the height of the canvas, the there is no padding applied to the bottom of the view.<br><br>If the property is omitted a default value of 5 is applied to the `x` and `y` properties.
| `spacing`| This is an optional property used to set the spacing between widgets on the canvas. The `x` and `y` values determine the horizontal and vertical spacing. <br><br>If the property is omitted, the widget spacing defaults to 10<br><br>**Note:** The `numberOfColumns` property places a limit on the pixel size of each grid cell. The widget spacing is achieved by reducing the area within the grid cell occupied by the widget. This in turn means that the spacing cannot be set larger than the dimensions of one cell. A side effect of this is that the higher the `numberOfColumns` the smaller the maximum spacing can be between widgets.|
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
