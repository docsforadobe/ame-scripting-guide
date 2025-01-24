# ItemAddedToBatchEvent

`onItemAddedToBatch`

The event will be sent after a batch item has been created successfully.

Can be used for all FrontendScriptObject API's which creates a batch item.

## Examples

### onItemAddedToBatch Example

```javascript
var source = "C:\\testdata\\testmedia.mp4";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia.mp4"

var frontend = app.getFrontend();
if (frontend) {
  frontend.addEventListener("onItemAddedToBatch", function (eventObj) {
    $.writeln("Item added to Batch");
  });

  var batchItemSuccess = frontend.addItemToBatch(source);
  if (batchItemSuccess) {
    $.writeln(source, " has been added successfully");
  }
}
```
