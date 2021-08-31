# Change Log

## [4369-Master | 4364-Dev] - 2021-08-31

- Added: Delegate action: Communicate from a widget in a tab to the root compilation by the use of pipeline action delegation.
- Added: Tree widget: Search support. Search table widget can be configured with `searchTable`.
- Added: Modify action: removeFromArray `item` can contain any fields of the message payload.
- Added: Gettime action: `asEpoch` added so that e.g. a relative date time can be converted directly to a milliseconds to Epoch value.

## [4281-Master | 4288-Dev] - 2021-07-22

- Added: Plotly widget: `plotlyOptions` can now be provided via the datasource.
- Added: Chart widget: tag table now makes use of the Table widget. This makes it possible to show additional columns which helps the user finding the right tags.
- Added: Tabs widget: A single tab can be changed by a `modify` action without the whole Tabs widget being reinitialized.
- Added: Modify action: The `modify` action now supports a `route` so that a widget on a tab can be addressed.

- Fixed: Table widget: Schema item style now overrules the widget options style settings.

- Warning: ReportViewer widget: don't specify `path` of the report object in the root of the widget model. Use `datasource` instead.

## [4194-Master | 4209-Dev] - 2021-06-07

- Added: Chart widget: Number of axis ticks can be set via the model.
- Added: Chart widget: Axis description, set via the model, is concatenated in the axis label.

- Fixed: Chart widget: User changes are now reflected in the work model.
- Fixed: Table widget: Row height issues with latest Chrome and Firefox version.

## [4105-Master | 4152-Dev]

- Added: Form widget: `showRefreshButton` option.

- Fixed: Table widget: Incorrect option 'refreshButton' is renamed to `showRefreshButton`.

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
