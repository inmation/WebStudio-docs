# Change Log

## [4136-Dev]

- Added: Tree widget.
- Added: Tabs widget (Preview).
- Added: Code completion in the model editor.
- Added: Compilations loaded from the system will hide the development tools (Add, Import, Export and {}).
- Added: gettime action to convert timestamps.
  
- Fixed: Table widget: Column filtering on formatted data.
- Fixed: Table widget: prevent sub rows from collapsing after an editable cell has been changed in a sub row.

- Removed: Victory widget is removed. Use Plotly widget.

## [3907-Master | 3900-Dev] - 2021-03-17

- Added: Chart widget has now a `modelRoot` property to specify the start node in the KPI Model.
- Added: Table widget: Support to configure the width for columns.
- Added: Report Viewer widget: Support for changing the execution context (ctx).
- Added: Plotly widget: Supports `onCLick` action hooks.

- Fixed: Subscription of the Chart widget could de-subscribe others like for the Faceplate widget.
- Fixed: Message Debugger not showing topic.
- Fixed: Action without a `type` is not treated as a passthrough.
- Fixed: Table: Filter input boxes are to big.
- Fixed: Report Viewer widget does not have vertical scroll bar.
- Fixed: Table widget: Filter input boxes are to big.
- Fixed: Table widget: Setting 'editable' to true for hierarchical table returns an error.
- Fixed: Text widget cannot show boolean value via dataSource.
- Fixed: A send action to or modify action of 'self' inside a life cycle hook results in an endless loop.

- Improved: Show error when widget type is invalid.
- Improved: Compilation and widget Diff Editor improvements.
- Improved: Upgrade and lazy loading of the Stimulsoft third party library.

## [3854] - 2021-02-26

- Fixed context of Report Viewer widget.
- Fixed table row selection when `multiMin` and `multiMax` are set.
- Added Plotly widget.
- Added support for hierarchical data in table widget.
- Added `addCursor` and `aetCursor` topics for Chart widget.

## [3672] - 2021-01-28

- Added `sort` schema property.
- Added `value` schema property.
- Added support for date range rules.

## [3655] - 2021-01-22

- Added `switch` action.
- Added `refresh` action.

## [3610] - 2020-12-21

- Added `convert` action.
- Added `mergeObjects` behavior to the `modify` action.
- Added Message Debugger widget. Replaces the need to use the Editor for message inspection.
- Added `onSelectionChanged` on Table widget.
- Added `update` topic for `send` action.
- Removed `completeMsgObject` from Editor widget. Use Message Debugger widget.

**Breaking change:**

- The `send` action cannot be used anymore to modify the widget model. Use `modify` action instead.

## [3428] - 2020-12-04

- Release with system version 1.72.0

## [3407] - 2020-12-03

- Loading WebStudio compilations from object custom properties is improved.
- Release candidate

## [3224] - 2020-11-20

- Beta version.
