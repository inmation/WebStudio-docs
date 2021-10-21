# Form
This widget is used to show a form which contains different types of entry fields used to accept data from the user.

## Model

```jsonc
    {
        "type": "form",
        "entries": [], // List of input fields
        "options": {}
    }
```

### Options

```json
{
    "options": {
        "showRefreshButton": true,
        "showToolbar": true,
        "refreshInterval": 30
    }
}
```

| name | description |
| ---- | --- |
| `showRefreshButton` | Set this to `false` to hide the refresh button. By default the refresh button is visible.
| `showToolbar` | to hide the complete toolbar.
| `refreshInterval` | Refresh with an interval in seconds.

### Actions

Supported actions:

- `onSubmit`: Gets invoked when the `Submit` button is clicked.

#### onSubmit

When the `onSubmit` action is defined in the `actions` a `Submit` button, will be shown in the bottom right corner of the form.

```jsonc
{
    "actions": {
        "onSubmit": [
            {
                "type": "function",
                "lib": "library-name",
                "func": "function-name",
                "farg": {} // This will be set automatically
            }
        ]
    }
}
```

The `onSubmit` action gets invoked when the `Submit` button is clicked. The input message `payload` is an array containing the `id` and `value` of each input entry. If the input entry contains more than one input values, the `value` will be an array or values.

```json
[
    {
        "id": "SingleSelect",
        "value": 2
    },
    {
        "id": "MultiSelect",
        "value": [
            1,
            2
        ]
    }
]
```

### Entries
The entries section is used to define the form fields. The common model for all field types is shown below. 

```jsonc
{
    "type": "input", 
    "id": "fld1",
    "description": "Field input details",
    "label": "Input 1",
    "value": "default value",
    "disabled": true
}
```

| Name | Description |
| --- | --- |
| `types` | Input field type. Supported options are:<br><ul><li>`buttons`: Radio buttons used to select one option from the predefined set.<li>`date`: Date-time input fields. The value can be typed in or selected from a date picker.<li>`input`: standard text input field.<li>`select`: Select one or more values from predefined drop-down options |
|`id` | Optional id field that can be used to locate a specific value in the [submitted](#onsubmit) data |
|`label` | Optional field label shown in bold above the field. |
|`description` | Optional field used to describe what input is expected for the field in question. If provided will appear between the `label` and input field. |
|`value` | Optional default value for the field. |
|`disabled` | Used to grey out the field and prevent the value from being changed |


Depending on the field type specified, some additional properties can also be set: 
#### Buttons
An example of the model for the button input field is shown below. In addition to the common properties it requires an `items` list containing the available radio buttons.    

```json
{
    "id": "Button01",
    "type": "buttons",
    "label": "Control",
    "description": "Control the equipment",
    "value": 1,
    "items": [
        {
            "color": "green",
            "label": "On",
            "value": 1
        },
        {
            "color": "yellow",
            "label": "Start",
            "value": 2
        },
        {
            "color": "blue",
            "label": "Stop",
            "value": 3
        },
        {
            "color": "grey",
            "label": "Off",
            "value": 4,
            "disabled": true
        }
    ]
}
```

> **Note**: The `value` property is compulsory for the button field. It can, but need not, be set to one of the item values, in which case the form will start out with that item being selected.

Item fields: 

| Name | Description |
| --- | --- |
|`color` | Optional button color. If omitted the border and selected background color will default to "white", which works will for dark theme compilations, but not for the light theme, where an explicit color should be provided (such ag "gray").|
|`label` | Button label |
|`value` | Compulsory item value. The value of the selected item is returned when the from is submitted, or the default field value if none of te buttons are selected. Also note that the types of the item values don't need to be the same. |
|`disabled`| Optional field to used to disable an item, preventing it from being selected. Note that the default value may be set to that of a disabled button.

#### Date Picker Entry
The model for a Date entry field is:

```jsonc
{
    "id": "DatePicker01",
    "type": "date",
    "label": "Start time",
    "description": "The start time of the interval to retrieve data for.",
    "value": "2020-11-01T01:00:00.000Z",
    "convertRelativeDate": false // Date specific field 
}
```
Using the date input field, a date/time can be selected from a pup-up date picker, invoked by clicking on the clock icon at the right edge of the field, or it can be directly entered. The date-time selected or entered is relative to the timezone of the browser. When the form is submitted, the date will be converted to an ISO UTC string. 

It is also possible to specify a relative time expressions (see [gettime](../../actions/README.md#gettime)). When the form is submitted, the relative time expression is converted to the equivalent explicit time string by default. The relative time string can also be passed through unchanged by setting `convertRelativeDate` to false. 

#### Input Entry
Generic input box. An input entry can be made readonly by setting `readonly` to true. A unit of measure can be defined by assigning it to the `uom` property.

```jsonc
{
    "id": "01",
    "type": "input",
    "label": "Pressure",
    "description": "Set the pressure value",
    "uom": "bar", // Optional unit of measure.
    "value": 0.5,
    "readonly": false // Optional, by default false
}
```

> **Note**: Irrespective of the `value` property's initial type, when a field is edited, the value in the submit array will always be a string! 
#### Select Entry
Drop-down entry field with the following model:

```jsonc
{
    "id": "Select01",
    "type": "select",
    "description": "Select a number or string",
    "label": "Select Test 1",
    "value": "10",
    "multi": false, // Optional, can be used to enable multi select, by default false.
    "free": false, // Optional, can be used to allow free input, by default false.
    "items": [
        {
            "label": "Number 1",
            "value": 1
        },
        {
            "label": "String 10",
            "value": "10"
        },
        {
            "label": "String Hello",
            "value": "Hello"
        },
        {
            "label": "String World",
            "value": "World"
        }
    ]
}
```
The type specific fields are:
| Name | Description |
| --- | --- |
| `items` | List of items to show in the dropdown. This list can either be make up of atomic values such as strings and number or each entry can be an object with the following fields:<ul><li> `label`: Optional display label used instead of the value. <li> `value`: Value returned by `onSubmit` when the item is selected. As is the case for the "button", the type of the value field can be different for each item.<li>`disabled`: Items may be disabled, in which case they will not be shown in the dropdown |
|`free` | If set, free form text can be entered, otherwise only options which appear in the drop-down can be selected.|
|`milti` | When set to `true`, multiple items can be selected. In this case, onSubmit will return an array of values.|

Default selected inputs can be defined by setting the `value` field at entry level. It can be a single value or an array of values and should match the value(s) of a predefined inputs. If the default `value` is defined as an array and `multi` is set to false, the first non `disabled` option will be selected.
