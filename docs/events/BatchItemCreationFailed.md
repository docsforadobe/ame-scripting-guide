# BatchItemCreationFailed Event

`onBatchItemCreationFailed`

The event will be sent after batch item creation failed.

Can be used for the following FrontendScriptObject APIs:

- `addFileToBatch`
- `addTeamProjectsItemToBatch`
- `addDLToBatch`

### Properties

|   Property    |  Type  |     Description      |
| ------------- | ------ | -------------------- |
| `error`       | String | The error string     |
| `srcFilePath` | String | The source file path |

### Example

```js
var source = "C:\\testdata\\testmedia.mp4";

// sources for mac
// var source = "/Users/Shared/testdata/testmedia.mp4"

var frontend = app.getFrontend();
if (frontend) {
  frontend.addEventListener("onBatchItemCreationFailed", function (eventObj) {
    $.writeln("Sourcefile", eventObj.srcFilePath);
    $.writeln("onBatchItemCreationFailed: error", eventObj.error);
  });

  var batchItemSuccess = frontend.addItemToBatch(source);
  if (batchItemSuccess) {
    $.writeln(source, " has been added successfully");
  }
}
```
