# Change Log

## [????]

- Victory widget removed. Use Plotly widget.

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
