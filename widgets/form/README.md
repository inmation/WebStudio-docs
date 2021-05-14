# Form

This widget can be used to show a form which can contain different type of entries in order to accept input data from the user.

## Model

```json
    {
        "type": "form",
        "entries": []
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

When the `onSubmit` action is defined in the `actions` a `Submit` button, will be shown at the bottom right corner of the form.

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

The `onSubmit` action gets invoked when the `Submit` button is clicked. The input message `payload` is an array containing the `id` and `value` of each input entry. In case the input entry contains more than one input values, the `value` will be an array containing the selected values.

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

Different type of entries in order to accept input data from the user.

- `buttons`: Allows for input by clicking one of the predefined buttons.
- `date`: Allows for a datetime input by selecting a date in a date picker.
- `input`: Allows for input by typing any value in an input box.
- `select`: Allows for input by selecting predefined inputs from a Drop-down list.

#### Buttons

Allows for input by clicking one of the predefined buttons. Buttons need to be defined in `items`. Each button requires a `label` and `value`. Optionally a button can be disabled by setting `disabled` to `true`. The `border color` and `selected color` can be configured by setting the `color`. The default color is `white`.

Default selected buttons can be defined by setting the `value` on entry level. This `value` can be a single value or an array of values. A default value should match the value of a predefined button.

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

#### Date Picker Entry

Allows for a datetime input by selecting a date in a date picker. A default datetime can be defined by setting the `value`.

```json
{
    "id": "DatePicker01",
    "type": "date",
    "label": "Start time",
    "description": "The start time of the interval to retrieve data for.",
    "value": "2020-11-01T01:00:00.000Z"
}
```

#### Input Entry

Allows for input by typing any value in an input box. An input entry can be made readonly by setting `readonly` to true. A unit of measurement can be defined for the input by setting `uom` in the entry.

```jsonc
{
    "id": "01",
    "type": "input",
    "label": "Pressure",
    "description": "Set the pressure value",
    "uom": "bar",
    "value": 0.5,
    "readonly": false // Optional, by default false
}
```

#### Select Entry

Allows to select one or more predefined input values. To allow for multiple values, `multi` needs to be set to `true`. Predefined inputs need to be defined in the `items` array. This can be an array of strings or an array of objects containing a `label` and `value`. The latter can be used when the `label` to show should be different than the `value`. Optionally a predefined input can be disabled by setting `disabled` to `true`.

Default selected inputs can be defined by setting the `value` on entry level. This `value` can be a single value or an array of values. A default value should match the value of a predefined input. In case the default `value` is defined as an array and `multi` is set to false, the first non `disabled` option will be selected.

To allow any input value besides a pre defined select input, the `free` field needs to be set to `true`.

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
