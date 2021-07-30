# Action Compilation

In this section you can find compilation examples of actions. Examples are showing how and to use specific actions.

| Examples | Description | Screenshot |
| --- | --- | --- |
| [cellClick-01](./cellClick-01.json)| Click on a cell in a table to executes a named action. |![cellClick-01](../../../assets/images/compilations/actions/Actions_01_cellClick-01.png)|
| [collect-selected-rows-01](./collect-selected-rows-01.json) | Click a Text widget to executes a collect action on selected table rows.|![collect-selected-rows-01](../../../assets/images/compilations/actions/Actions_02_collect-selected-rows-01.png)|
| [collect-table-data-01](./collect-table-data-01.json) | Click a Text widget to collect the entire table dataset and combine this with the initial message payload.|![collect-table-data-01](../../../assets/images/compilations/actions/Actions_03_collect-table-data-01.png)|
| [consolelog-01](./consolelog-01.json) | Click to executes a `consoleLog` action. The Text widget payload is logged to the console.|![consolelog-01](../../../assets/images/compilations/actions/Actions_04_consolelog-01.png)|
| [consolelog-02](./consolelog-02.json) | Click to executes a `consoleLog` action. The Text widget payload is logged to the console with a tag.|![consolelog-02](../../../assets/images/compilations/actions/Actions_05_consolelog-02.png)|
| [consolelog-03](./consolelog-03.json) | Click to executes a `consoleLog` action. Custom payload is logged in the console with a tag.|![consolelog-03](../../../assets/images/compilations/actions/Actions_06_consolelog-03.png)|
| [consolelog-04](./consolelog-04.json) | Click on a cell executes a `consoleLog` action. Message payload is merged with key-value data and logged in the console with a tag.|![consolelog-04](../../../assets/images/compilations/actions/Actions_07_consolelog-04.png)|
| [copy-01](./copy-01.json) | Click on a cell copies text.|![copy-01](../../../assets/images/compilations/actions/Actions_08_copy-01.png)|
| [copy-02](./copy-02.json) | Click on a cell copies text and notifies.|![copy-02](../../../assets/images/compilations/actions/Actions_09_copy-02.png)|
| [function-01](./function-01.json) | Click on a cell invokes advanced endpoint.|![function-01](../../../assets/images/compilations/actions/Actions_10_function-01.png)|
| [invoke-action-by-name-01](./invoke-action-by-name-01.json) | Click invokes named actions. The first is defined on the Text widgets. This named action invokes a second defined at the compilation level.|![invoke-action-by-name-01](../../../assets/images/compilations/actions/Actions_11_invoke-action-by-name-01.png)|
| [modify-01](./modify-01.json) | Click to modifies text size and color.|![modify-01](../../../assets/images/compilations/actions/Actions_12_modify-01.png)|
| [modify-02](./modify-02.json) | Click to modifies text size.|![modify-02](../../../assets/images/compilations/actions/Actions_13_modify-02.png)|
| [modify-03](./modify-03.json) | Click to sets the text style to default.|![modify-03](../../../assets/images/compilations/actions/Actions_14_modify-03.png)|
| [modify-04](./modify-04.json) | Click to changes the aggregation of the pen in the chart.|![modify-04](../../../assets/images/compilations/actions/Actions_15_modify-04.png)|
| [modify-05](./modify-05.json) | Click to adds a new row to the table.|![modify-05](../../../assets/images/compilations/actions/Actions_16_modify-05.png)|
| [modify-06](./modify-06.json) | Click to remove a row from the table.|![modify-06](../../../assets/images/compilations/actions/Actions_17_modify-06.png)|
| [modify-07](./modify-07.json) | Click to remove a table row where the match condition is stated as "<column-name>" : "<cell-value>" .|![modify-07](../../../assets/images/compilations/actions/Actions_18_modify-07.png)|
| [modify-08](./modify-08.json) | Click to remove a table row where the match condition is expressed as an array of { "name" : "<column-name>", "value" : "<cell-value>" } objects.|![modify-08](../../../assets/images/compilations/actions/Actions_19_modify-08.png)|
| [modify-09](./modify-09.json) | Click to filters out table rows based on a greater then or equal to criteria.|![modify-09](../../../assets/images/compilations/actions/Actions_20_modify-09.png)|
| [notify-01](./notify-01.json) | Click to invoke a notification action.|![notify-01](../../../assets/images/compilations/actions/Actions_21_notify-01.png)|
| [openlink-01](./openlink-01.json) | Click to open a hypermedia link.|![openlink-01](../../../assets/images/compilations/actions/Actions_22_openlink-01.png)|
| [passthrough-01](./passthrough-01.json) | Click to passes input message payload to the next action in the pipeline, which in this case is the browser consoles.|![passthrough-01](../../../assets/images/compilations/actions/Actions_23_passthrough-01.png)|
| [prompt-01](./prompt-01.json) | Click to show a prompt dialog containing a text widget.|![prompt-01](../../../assets/images/compilations/actions/Actions_24_prompt-01.png)|
| [read-lua-kpi-table-01](./read-lua-kpi-table-01.json) | Click to retrieves a KPI table.|![read-lua-kpi-table-01](../../../assets/images/compilations/actions/Actions_25_read-lua-kpi-table-01.png)|
| [read-object-value-01](./read-object-value-01.json) | Click to retrieve data from the read endpoint.|![read-object-value-01](../../../assets/images/compilations/actions/Actions_26_read-object-value-01.png)|
| [read-write-01](./read-write-01.json) | Table data is retrieved using read endpoint, click on save button to write data to the endpoint|![read-write-01](../../../assets/images/compilations/actions/Actions_27_read-write-01.png)|
| [send-01](./send-01.json) | Click to send data to editor widget.|![send-01](../../../assets/images/compilations/actions/Actions_28_send-01.png)|
| [send-selectRows-01](./send-selectRows-01.json) | Click to select table rows using a send action.|![send-selectRows-01](../../../assets/images/compilations/actions/Actions_29_send-selectRows-01.png)|
| [switch-01](./switch-01.json) | Click on a table cell in a specific column to execute an action based on a matching case condition. Also illustrates the use of multiple pipelines.|![switch-01](../../../assets/images/compilations/actions/Actions_30_switch-01.png)|
| [transform-example-01](./transform-example-01.json) | Transform input using MongoDB's Aggregation Pipeline's $project logic. Illustrates the use of the Transform Editor|![transform-example-01](../../../assets/images/compilations/actions/Actions_31_transform-example-01.png)|
| [transform-example-02](./transform-example-02.json) | Transform input using MongoDB's Aggregation Pipeline's $match logic.|![transform-example-02](../../../assets/images/compilations/actions/Actions_32_transform-example-02.png)|
| [transform-example-03](./transform-example-03.json) | Filter input based on key value.|![transform-example-03](../../../assets/images/compilations/actions/Actions_33_transform-example-03.png)|
| [transform-example-04](./transform-example-04.json) | Filter input based on key value and return first matching result.|![transform-example-04](../../../assets/images/compilations/actions/Actions_34_transform-example-04.png)|
| [transform-example-05](./transform-example-05.json) | Transform entire input using MongoDB's Aggregation Pipeline logic.|![transform-example-05](../../../assets/images/compilations/actions/Actions_35_transform-example-05.png)|
| [wait-example-01](./wait-example-01.json) | Click on a cell prompts notification with one second delay.|![wait-example-01](../../../assets/images/compilations/actions/Actions_36_wait-example-01.png)|
| [write-example-01](./write-example-01.json) | Click on a cell increases or decreases value of inmation object.|![write-example-01](../../../assets/images/compilations/actions/Actions_37_write-example-01.png)|
