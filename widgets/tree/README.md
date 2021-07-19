# Tree

The Tree widget can be used to show hierarchical data in the form of nodes. Each node can have a sheer number of icons.

## Model

```jsonc
{
    "type": "tree",
    "data": [
        {
            "n": "node-01",     // Shown as the label of the node
            "c": []             // Children             
        }
    ],
    "schema": []                // See Schema
}
```

### Schema

In order to support data with different fields for the standard `n` and `c`, a mapping can be defined pointing to custom fields of your node data.

```jsonc
{
    "schema": {
        "name": "myNodeTitle",
        "children": "myListOfChildren"
    }
}
```

Assigning icons to nodes can be done via the `schema`. Nodes can have an arbitrary number of icons. By default the icons are positioned to the left of the node name. Icons can also be assigned on the right side of the node name. This can done by providing an `alignment` field with the value `leading` or `trailing` and a `position` field in the icon definition. With `rules` you can assign a specific icon depending on the data of a node.

Icon position numbering:

| leading | node label | trailing |
| --- | --- | --- |
3 <- 2 <- 1 | MyNode | 1 -> 2 -> 3

Icons can be defined by:
| Type | Example |
| --- | --- |
| Emoji | "🌍" |
| URL to image | { "url": "" } |
| Base64 encoded | { "mimeType": "image/jpg", "base64": "" } |

```jsonc
{
    "schema": {
        "name": "n",                            // Default name field is 'n'
        "children": "c",                        // Default children field is 'c'
        "icons": [                              // Generic for all nodes
            {
                "alignment": "leading",        // Default is 'leading'
                "position": 1,                 // Default index in the array + 1
                "icon": "ℹ️",
                "actions": {}
            },
            {
                "alignment": "leading",
                "position": 2,
                "icon": "❎",
                "actions": {}
            }
        ],
        "style": {                              // Node styling options
            "fontSize": "15px"
        },
        "rules": [                              // Node specific icons, actions and styling
            {
                "match": {},                    // Match query
                "actions": {},                  // Node actions
                "icons": [
                    {
                        "alignment": "leading",
                        "position": 2,
                        "icon": "🔍",
                        "actions": {}           // Action actions
                    }
                ],
                "style": {                      // Node styling options
                    "backgroundColor": "green",
                    "fontSize": "15px",
                    "fontWeight": "normal"
                }
            }
        ]
    }
}
```

The `schema` in the model will overrule the `schema` received from the data source fetch. By adding `extendSchemaFromDataSource` to the `schema` in the model you can make use of both. Typically scenario is that the system returns `data` containing the node structure and a `schema` containing icons and that the model `schema` has custom `actions`.

This `extendSchemaFromDataSource` property only applies for `schema` configuration which are defined in the model.

```json
{
    "schema": {
        "extendSchemaFromDataSource": true
    }
}
```

### Options

Node styling and icon assignment can be combined using the `rules` in the `options`. The configuration in the `schema` is dominant to this `options`.

```jsonc
{
    "options": {
        "rules": [
            {
                "match": {},
                "icons": [                      // List of icon definition
                    {
                        "icon": "ℹ️",
                        "actions": {}           // See Actions
                    },
                    {
                        "icon": "👁",
                        "actions": {}           // See Actions
                    }
                ],
                "style": {
                    "backgroundColor": "green",
                    "fontSize": "15px",
                    "fontWeight": "normal"
                }
            },
            {
                "match": {},
                "icons": [                      // List of icon definition
                    {
                        "alignment": "trailing",
                        "position": 1,
                        "icon": "🌍",
                        "actions": {}           // See Actions
                    }
                ],
                "style": {
                    "fontWeight": "bold"
                }
            }
        ]
    }
}
```

### Actions

Nodes and icon of nodes support action hooks. An action hook supports the standard action pipeline. The message payload is de data of the node.

Supported action hooks:

- `onClick`: when user clicks on the node or on one of its icons.

Generic `onClick` action hook for a node:

```json
    {
        "actions": {
            "onClick": {
                "type": "send",
                "to": "debugger"
            }
        }
    }
```

Generic `onClick` action hook for a node icon:

```jsonc
{
    "icons": [
        {
            "icon": "ℹ️",               // Only applies when there is no matching rule.
            "actions": {
                "onClick": {
                    "type": "send",
                    "to": "debugger"
                }
            }
        }
    ]
}
```

A specific action can be configured within the rules. These are dominant to the generic actions.

Action on a specific icon:

```jsonc
{
    "icons": [
        {
            "icon": "ℹ️",               // Only applies when there is no matching rule.
            "rules": [
                {
                    "match": {
                        "t": 1034       // Type in the form of the class number
                    },
                    "icon": "🍺",
                    "actions": {
                        "onClick": {
                            "type": "send",
                            "to": "debugger"
                        }
                    }
                }
            ]
        }
    ]
}
```
