# Chart Compilations

| Examples | Description |
| --- | --- |
| [dynamic-chart-01](./dynamic-chart-01.json)| Select object to be added to chart.
| [set-add-clear-cursors-01](./set-add-clear-cursors-01.json)| Click on a cell to set, add or clear cursors. Uses a pre-set time period for the chart data and cursors (which may be older than what you have data for in your system. If this is the case, also look at the next example to see the effects of the set and add cursor send actions using recent data.)
| [set-add-clear-cursors-02](./set-add-clear-cursors-02.json)| Click on a cell to set, add or clear cursors. This example is the same as the one above, but gets data for the most recent hour instead of using a fixed time range. As a consequence, the cursor time-stamps also need to be set based on the current time. The `gettime` action is used to start the process. The returned time-string is then parsed and assigned to a timestamp property in the payload of the message such that it can be sent to the chart's widget cursors array. 
