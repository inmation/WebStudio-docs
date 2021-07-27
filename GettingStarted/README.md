# Getting Started

The aim of the "Getting Started" section is to convey the key WebStudio concepts and to, as far as possible, provide a more or less linear path to learning the tool. To get the best out of this guide, it is recommended that you work along and try things out for yourself. To do so, you'll need access to a running inmation instance and have login credentials to connect to the core. Also make sure you have installed the `Examples_WebStudio_Demo_Data_V<n.nn>.json` as described on the [WebStudio root](../README.md) page.

Let's quickly recap the main points from [earlier](../README.md):

- The WebStudio application is used to host interactive views, called [compilations](../referencedocs/README.md#compilation), made up of one or more widgets.
- The application runs in your browser of choice and is loaded using a URL that looks something this
```url
    http://<hostname_webapi>:8002/apps/webstudio/
```
- Starting from a blank canvas, you can create your own views by adding `widgets` to the compilation. 
- Widgets are able to bind to, and display dynamic data from the core inmation server and can be configured to react to user interactions. 
- Complications and the widgets contained within are defined using JSON (JavaScript Object Notation)

## Compilations basics

### Work-Space and Grid settings
To get us going, we'll create a "Hello World" compilation, consisting of a single text widget placed in the middle of the workspace. Load the application and make sure you are logged into the back end. At this point you should see the workspace with the developer toolbar icons displayed at the top. 

![WebStudio - Menubar](../assets/images/webstudio-menubar.png)

The work area or canvas starts off divided into a rectangular grid of 24 columns across. When placing widgets onto the canvas they will always align to the grid and their sizes will be multiples of the grid vertical and horizontal spacing. 

As mentioned, compilations are defined using JSON text, which can seem a bit daunting at first. While the [syntax](https://www.json.org) is not too hard to follow, knowing what structure and parameter to use is not immediately obvious. Fortunately the built-in templates and editor code-completion take care of a lot of the heavy lifting for us, and if all else fails, there is always the online documentation to fall back on.

Click on the **`{}`** icon in the toolbar to show the compilation in the JSON editor. 

> **Note**: Throughout the documentation, reference is made to "Models" to refer to the set of JSON objects that define elements of the compilation or even the compilation as a whole. In the context of individual widgets, the [reference pages](../ReferenceDocs/widgets/README.md) all contain a "Model" sections describing the characteristic - minimum configurations - for the item in question. In almost all cases the actual model used will consist of the base config combined with common settings such as options and data-sources etc ... 

The default JSON for a blank compilation looks something like this. 

```json
{
    "version": "1",
    "widgets": [],
    "options": {
        "stacking": "vertical",
        "numberOfColumns": 24
    }
}
```

The default [options](../ReferenceDocs/README.md#options) are set such that any widgets added to the compilation will be stacked vertically, starting at the top of the window. For our first example, we'd like to put the text box in the middle of the canvas, so let's change `stacking` property. Delete the `"vertical"` setting, press **Control-Space** to bring up a list of valid stacking options, then select `"none"` which will allow widgets to be place in any grid cells on the screen. 

In general the **Control-Space** keyboard shortcut can be used to show what properties (and possible values in some cases) are applicable within the various sections of the model. To see this in action, place the edit cursor at the end of the line ```"stacking":"none",``` and press **Control-Space** again to be prompter with the available, as yet un-used, options. 

> **Note:** Entering a **space** in the editor when the cursor is placed in a position where it is valid to add new properties will also show the code-completion pop-ups. The difference between this and hitting **Control-Space** is that the latter also works while you are editing object properties and values (inside string double quotes).  

We'll return to the compilation in a bit. For now, close the editor and click on the **`+`** icon at the top of the browser window to bring up the widget editor which presents a palette of pre-configured templates on the left. Expand the "Text" tree node and select the "Simple Text" option, to load some boilerplate settings. From here we can start customizing the widget's appearance and behavior. Let's change the value of the `text` property to "Hello World". 

Next we'll adjust the text widget to be two grid cells high and six wide. Move your edit cursor to the end of the line that now reads: 
```json 
"text": "Hello World", 
``` 
and press the space bar. From the available options, select `layout`, which inserts a JSON fragment.

![Text options](../assets/images/webstudio-code-completion.png)

Press the space bar again to see the available `layout` fields and set `"h"=2,` and `"w"=6`. At this point the JSON formatting is a out of kilter, which we can fix by clicking the format ![brush](../assets/images/FormatJSONToolBtn.png) icon at the top of the window. As a last flourish before adding the widget, let's change the text `color`. Place the cursor inside the current value (`"grey"`) of the `options.style.color` field and press **control-space** and pick a color.   

> **Note**: Dot-notation is used as a shorthand to refer to nested properties in the JSON models as shown above in relation to setting the `color` attribute. The notation is fairly self-explanatory. One special case worth mentioning more explicitly is when referring to objects in arrays. Consider the arbitrary JSON below:
>```json
>{
>    "data": [
>        {
>            "x": 0,
>            "y": 1.2
>        },
>        {
>            "x": 1,
>            "y": 3.8
>        }
>    ]
>}
>```
>
>To refer to the y-value of the second data point, the notation used is `data.1.y` 


Apply the selection, by clicking on the save icon, drag the widget to the center of the canvas. 

![Hello World](../assets/images/webstudio-HelloWorld.png)

### Widget title-bar and resize handles

To finish things off, let's remove the title-bar and resize drag handle from the widget. Click the `{}` in the widget title-bar and set the `captionBar.hidden` field to `true`, then apply the change to see what it looks like... 

So far so good, except... how do we now get back to the JSON for the widget to make further changes? The answer is to use the compilation editor. Click the `{}` at the top of the work area and locate the text widget in the compilation's `widgets` array. 

In this case, it is easy to do since there is only one widget in the compilation, but in general finding your widget in the array will be more cumbersome. There are two strategies that may help to mitigate the problem: 

- Use the editor's "Find" function (**Control-F** or **⌘-F** on Mac) to search for a string you know to be part of the widget you are looking for. 
- Use the editor's code-folding arrow buttons in the left sidebar of the editor to collapse sections of the JSON you want to ignore. 

![Code-folding](../assets/images/webstudio-codefolding.png)

With the compilation JSON loaded in the editor, change the `widgets.0.layout.static` property to `true` and apply the changes to remove the re-size drag handle, which leaves us with the final result:

![Clean-text-widget](../assets/images/webstudio-cleanTextWidget.png)

... OK, that was somewhat underwhelming, so let's pick up the pace a bit. (**Note**: The application and web-studio version numbers that normally appear next to the inmation logo have been obscured in the screenshots)

Before moving to the next section here are a few things you can try on your own:
> **Things to try**:
> - Add one instance of each of the templated widgets to your compilation,  to get an impression of what they look like. 
>
>> **Note** Most of the template widgets are pre-configured to bind to the example demo data mentioned above so they can show real values. If your inmation system was installed to use the host machine name as the name of the "Core" node in the IO-Model, you will need to update the bindings in the widgets config. (We'll return to the topic of [data-binding](#data-binding) in a bit more detail below)
>
> For example: If you add the `Faceplate` templates for tag `FC4711`, its default model JSON looks like this:
>```json
>{
>    "type": "faceplate",
>    "name": "FC4711",
>    "description": "Process Data FC4711",
>    "path": "/System/Core/Examples/Demo Data/Process Data/FC4711",
>    "captionBar": true,
>    "layout": {
>        "h": 4
>    }
>}
>```
>Looking in DataStudio you might find that the actual path to the referenced tag is something like "/System/*INMATION-HOST-01*/Examples/Demo Data/Process Data/FC4711". To make the binding work in WebStudio all path values then need to be  updated by replacing occurrences of /core/ with the name used (such as *INMATION-HOST-01* in the example). 
>
>In the editor press **^-F** (**⌘-F** on Mac) to show the **Find** option and enter the string to find.
>
>![Find](../assets/images/webstudio-editor-find.png)
>
>Press the **>** arrow in front of the find input box to show the **Replace** input box, enter the name of your Core node and click the "Replace All" button.
>
>![replace](../assets/images/webstudio-replace.png) 
>
>If the all went according to plan, the Faceplate should show up in the compilation with live data:
>
>![faceplate](../assets/images/webstudio-Faceplate.png)
>
> - Experiment with the code-completion options to see which config options are
> available in addition to the 'out-of-the-box' templates settings. 

## Displaying inmation data
The reason for creating compilations is mostly to display core data to platform users. In some scenarios modifications may need to be made by editing, adding or deleting information. We'll cover the second scenario later. For now, the focus is on how to retrieve and display information. 

### Path based bindings
The `path` of object in the various models, typically IO and KPI, is how they are uniquely identified in the system and is therefore used to configure direct bindings in WebStudio.

Where in the model the `path` is specified is somewhat Widget dependent. Consider the following examples:

- `Faceplate`: Here the path is a base property of the widget
    ```json
        {
            "type": "faceplate",
            "name": "DC4711",
            "description": "Process Data DC4711",
            "path": "/System/Core/Examples/Demo Data/Process Data/DC4711",
            "captionBar": true,
            "layout": {
                "h": 4
            }
        }
        ```
- `Chart`: The chart is quite a complex widget and is able to retrieve core data in a number of ways, the most direct of which is to define one or more pens and to set their path properties. 
    ```json
    {
        "type": "chart",
        "chart": {
            "pens": [
                {
                    "path": "/System/Core/Examples/Demo Data/Process Data/DC4711",
                    "aggregate": "AGG_TYPE_INTERPOLATIVE", ...
                }
            ],
            "x_axis": [...],
            "y_axis": [...],
        },
        "name": "Webchart with pen",
        "description": "Webchart with pen",
        "layout": {...}
    }

    ```

### Data Source
Most widgets that display data can be configured to do so by specifying the appropriate [dataSource](../ReferenceDocs/Widgets/README.md#datasource) settings. The `dataSource` property supports a number of options which determine the binding behavior. Let's have a look at some of these applied to a `Text` widget:

> **Note**: The `Text` widget is used here to illustrate the concepts due to its simplicity rather than its inherit suitability for that purpose.

#### Read
When the `dataSource` type is set to `"read"` the value of the item pointed to by the `path` is read whenever the widget is refreshed, rather than when the underlying value changes. 
```json
{
    "type": "text",
    "text": "",
    "dataSource": {
        "type": "read",
        "path": "/System/Core/Examples/Demo Data/Process Data/DC4711"
    },
    "layout": {
        "w": 6,
        "h": 2
    },
    "id": "demoTxt"
}
```

> **NOTE** If you use this JSON, the tag value is shown with too many decimals. We'll fix that when we get round to talking about pipelines. 

The `"read"` dataSource type is therefore more often used for properties, such as the configured number of decimals on the tag or the engineering units, that tend to be static. (The engineering units of the tag can be read by appending the property name to the path as show)

```json
    {
        "type": "read",
        "path": "/System/Core/Examples/Demo Data/Process Data/DC4711.OpcEngUnit"
    },
```

#### Subscribe
In the example above, we could have "worked around" the fact that the data source is only read when the widget is refreshed by specifying a `refreshInterval` in the model, like so:

```json
{
    "type": "text",
    "text": "",
    "options": {
        "refreshInterval": 10
    },
    "dataSource": {
        "type": "read",
        "path": "/System/Core/Examples/Demo Data/Process Data/DC4711"
    },
    "layout": {
        "w": 6,
        "h": 2
    },
    "id": "demoTxt"
}
```

Now the source value will be polled once every ten second. While this works, it is still not ideal. If the source value changes once a second for example, we'll miss 90% of the updates.

A better approach is to change the dataSource type to `"subscribe"`. WebStudio will now be notified whenever the tag value we subscribed to changes. 

#### Function
Another binding option is to call an [advanced endpoint](https://inmation.com/docs/api/latest/webapi/advancedendpoints.html) to execute Lua script in the back end. This is achieved by setting the `dataSource` type to `"function"`. The ability to call custom code in the core provides a great deal of flexibility, opening the door to manipulating or even generating data as required. 

To see this in action, add a table widget to your compilation and select the "Data source function" template in the `Table` section. The JSON in the editor should looks something like this:

```json
{
    "type": "table",
    "description": "Data Source function",
    "name": "Demo Process Data",
    "dataSource": {
        "type": "function",
        "lib": "webstudio-demo",
        "func": "demoProcessDataTableData",
        "ctx": "/System/Core/Examples/WebStudio"
    },
    "schema": [ ... ],
    "captionBar": true,
    "options": { ... },
    "layout": { ... }
}
```

> **Things to try:**
> - Pick one of the widgets and experiment with the various data source types discussed. Try setting up our own core object and show their state in WebStudio
> - Look at all the templated to see where they get their data from. (You will need the demo dataset to see them in action)

We are not quite done with dataSources yet, but in order to complete the discussion we need to make a detour to talk about `Hooks`, `Pipelines` and `Actions`

## Hooks, Actions, Messages and Pipelines.
As we've seen so far, WebStudio takes JSON models and uses these to work out what to draw on the screen. In doing so, it goes through a set of phases during which the model is processed, data is retrieved, the UI is updated and so on. 

Webstudio provides life cycle hook to allow us to react to the start and end of each of the phases (don't worry if that statement doesn't make 100% sense yet, things should become clearer in a bit). 

### Action Hooks
In addition, and depending on the widget, there are also action-hooks that are triggered when a user interacts with the widget in some way, such as clicking on it for examples or submitting a form. 

To define what happens in response to any of the triggers firing, all widget can have an `actions` property configured. Let's look at an example...

Starting from a blank compilation, click the `+` toolbar options and paste in the JSON config for a `Text` widget.

```json
{
    "type": "text",
    "name": "Simple Text",
    "text": "Click Me",
    "actions": {
        "onClick": {
            "type": "notify"
        }
    },
    "captionBar": true,
    "layout": {
        "w": 8,
        "h": 2
    }
}
```

Looking at the JSON we see the `actions` property, contains an `onClick` object of type `notify`. When you click on the text, a "notification" is pop up in the top right corner of the workspace, displaying the widget's name: "Simple Text".

![onClick](../assets/images/webstudio-onclick.png)

Refer to the [Actions](../referencedocs/actions/readme.md#actions) help pages for a full list of all the action types supported.  

### Messages
One very important aspect of the way actions work is that they all receive a `Message` object as input, which in turn will always have at least one property called `payload`. The initial content of the payload is populated by the widget which triggered the action. It initializes the payload with values from its current state. We'll come back to this in more detail, but let's first see the `Message` in action.

Consider the compilation shown below. It has two "buttons" (text widgets with `onClick` actions) on the left. These set the background color of the "Status" text widget to red or green when clicked. 

![toggleBgColor](../assets/images/webstudio-toggle.png)

After clicking on "Green" for example, the view changes to:

![toggleBgColor](../assets/images/webstudio-toggle-green.png)

The actions configuration of the "Red button" is shown below (The compilation JSON is linked: [redgreen_v1.json](./compilations/gettingstarted/redgreen_v1.json) )

```json
{
    "type": "text",
    "text": "Red",
    "captionBar": false,
    "actions": {
        "onClick": {
            "type": "modify",
            "id": "indicator",
            "debug" : true,
            "set": [
                {
                    "name": "model.options.style.backgroundColor",
                    "value": "$payload"
                }
            ]
        }
    },
    ...
}
```

The action triggered by the `onClick` hook is of type [modify](../referencedocs/actions/readme.md#modify) which, as the name suggests, is used to make changes to widgets in the compilations. The `id` field identifies the target widget to be modified. 

In the modify action, multiple properties of the target widget can be changed at once. The `set` property, for example, is an array of { `name`, `value` } pairs indicating the fields to change and the values to set them to. 

Notice that there is also a `debug` property which is enabled in the example. It causes the `message` object to be written to the browser's console from where we can inspect the content. 

> To bring up the browser console window, **right-click** anywhere on a blank area of the workspace and select the "inspect" popup menu option from where you should be able to locate a "console" tab. For most browsers the console is a "developer mode" feature which may need to be enabled before it is available. A quick bit of "googling" for the term "show browser console" should take you to the detailed instructions to get at the console.

The `message` looks like this:

```json
{
    "model": {
        "captionBar": false,
        "description": "Simple Text",
        "id": "indicator",
        "name": "Simple Text",
        "options": {
            "style": {
                "color": "White",
                "textAlign": "center",
                "fontSize": "26px",
                "backgroundColor": "transparent"
            },
            "text": "Status",
            "type": "text"
        },
        "layout": { "x": 3,  "w": 3,  "h": 2,  "y": 0, "static": true }
    },
    "payload": "Red"
}
```
In addition to the `payload` property we see that there is also a `model` property  which is the JSON of the target widget. With the message in hand, we can now make sense of the `set` statement. 

- `"name"`: "model.options.style.backgroundColor" refers to the name of the property that will be modified by the action in dot-notation. 
- `"value"`: "$payload" assigns the value of the message `payload` field to the background property. The `$` in front of the field indicates that we need to resolve.   

<!-- TODO : Complete -->