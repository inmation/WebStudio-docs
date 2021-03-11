# Report Viewer

This widget can be used to show generated reports.

## Model

```json
{
    "type": "reportviewer",
    "path": "/System/Core/Examples/WebStudio/Reports/Process Data Report"
}
```

### Using DataSource

By configure a data source, a query can be provided. See [Report Item](#Report-Item) what needs to be done in the system in order to generate report data based on this query.

The refresh button allows to refresh report design and report data.

```json
{
    "dataSource": {
        "type": "fetch",
        "path": "/System/Core/Examples/WebStudio/Reports/Process Data Report",
        "query": {
            "batchid": "AB1234"
        }
    }
}
```

#### Execution Context

Default execution context is the path of the report item provided in the `dataSource`. Setting `ctx` value to `default` means that the default Web API context will be used, which is configured in the Web API server object. This is the same behavior as not providing the `ctx` option in an advanced endpoint request to the Web API. A custom execution context can be provided by setting the `ctx` field with a valid object path.

```json
{
    "dataSource": {
        "type": "fetch",
        "path": "/System/Core/Examples/WebStudio/Reports/Process Data Report",
        "query": {
            "batchid": "AB1234"
        },
        "ctx" : "default"
    }
}
```

### Stimulsoft Viewer Options

The viewerOptions of Stimulsoft Report Viewer can be set in the appearance object. The available options can be found at [Stimulsoft.Viewer.StiViewerOptions](https://admin.stimulsoft.com/documentation/classreference-js/classes/stimulsoft.viewer.stivieweroptions.html)

```json
{
    "viewerOptions": {
        "appearance": {
            "backgroundColor": {
                "name": "White",
                "a": 255,
                "r": 255,
                "g": 255,
                "b": 255
            },
            "pageBorderColor": {
                "name": "Gray",
                "a": 255,
                "r": 128,
                "g": 128,
                "b": 128
            },
            "rightToLeft": false,
            "fullScreenMode": false,
            "scrollbarsMode": false,
            "openLinksWindow": "_blank",
            "openExportedReportWindow": "_blank",
            "showTooltips": true,
            "showTooltipsHelp": true,
            "pageAlignment": 1,
            "showPageShadow": false,
            "bookmarksPrint": false,
            "bookmarksTreeWidth": 180,
            "parametersPanelPosition": 0,
            "parametersPanelMaxHeight": 300,
            "parametersPanelColumnsCount": 2,
            "parametersPanelDateFormat": "",
            "parametersPanelSortDataItems": true,
            "interfaceType": 0,
            "chartRenderType": 3,
            "reportDisplayMode": 3,
            "datePickerFirstDayOfWeek": 0,
            "allowTouchZoom": true,
            "htmlRenderMode": 3
        },
        "toolbar": {
            "visible": true,
            "displayMode": 0,
            "backgroundColor": {
                "name": "Empty",
                "a": 0,
                "r": 255,
                "g": 255,
                "b": 255
            },
            "borderColor": {
                "name": "Empty",
                "a": 0,
                "r": 255,
                "g": 255,
                "b": 255
            },
            "fontColor": {
                "name": "Empty",
                "a": 0,
                "r": 255,
                "g": 255,
                "b": 255
            },
            "fontFamily": "Arial",
            "alignment": 3,
            "showButtonCaptions": true,
            "showPrintButton": true,
            "showOpenButton": false,
            "showSaveButton": false,
            "showSendEmailButton": false,
            "showFindButton": true,
            "showBookmarksButton": true,
            "showParametersButton": true,
            "showResourcesButton": true,
            "showEditorButton": true,
            "showFullScreenButton": true,
            "showFirstPageButton": true,
            "showPreviousPageButton": true,
            "showCurrentPageControl": true,
            "showNextPageButton": true,
            "showLastPageButton": true,
            "showZoomButton": true,
            "showViewModeButton": true,
            "showDesignButton": false,
            "showAboutButton": true,
            "showPinToolbarButton": true,
            "printDestination": 0,
            "viewMode": 0,
            "multiPageWidthCount": 2,
            "multiPageHeightCount": 2,
            "zoom": 100,
            "menuAnimation": true,
            "showMenuMode": 0,
            "autoHide": false
        },
        "exports": {
            "storeExportSettings": true,
            "showExportDialog": true,
            "showExportToDocument": true,
            "showExportToPdf": true,
            "showExportToHtml": true,
            "showExportToHtml5": true,
            "showExportToWord2007": true,
            "showExportToExcel2007": true,
            "showExportToCsv": true,
            "showExportToText": true,
            "showExportToOpenDocumentWriter": true,
            "showExportToOpenDocumentCalc": true,
            "showExportToPowerPoint": true
        },
        "email": {
            "showEmailDialog": true,
            "showExportDialog": true,
            "defaultEmailAddress": "",
            "defaultEmailSubject": "",
            "defaultEmailMessage": ""
        },
        "viewerId": "",
        "reportDesignerMode": false,
        "requestResourcesUrl": "",
        "requestStylesUrl": "",
        "actions": {
            "exportReport": 1,
            "emailReport": 2
        }
    }
}
```

## Report Item

In order to generate a report based on query parameters from the widget, the following Lua script needs to be stored in the `Lua Script Body` of the `Report Item`. The function argument follows the same principle as the Advanced Endpoint feature of the Web API.

```lua
local JSON = require('rapidjson')

local function getReportData(batchid)
    -- Generate data for the report.
end

return function(arg, req, hlp)
    if arg then
        -- Is invoked via the Web API.
        local query = arg.query
        local reportData
        if type(query) == 'table' then
            -- e.g. batchid provided in the query.
            reportData = getReportData(query.batchid)
        else
            -- No query provided.
            reportData = getReportData()
        end
        return reportData
    end
    -- Is invoked within the system.
    local reportData = getReportData()
    return JSON.encode(reportData)
end
```
