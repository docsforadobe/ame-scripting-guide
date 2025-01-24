# EncoderHostWrapperEvent

Provides the following event types for items in the batch queue:

- `onItemEncodingStarted`
- `onAudioPreEncodeProgress`
- `onEncodingItemProgressUpdate`
- `onItemEncodeComplete`

For multiple batch items in the queue we recommend to use this event to ensure that the event types will be received for all batch items.

It provides the following event type for the whole batch queue:

- `onBatchEncoderStatusChanged`

---

## Properties

|            Property            |      Type       |                                                                                                                                                    Description                                                                                                                                                     |
| ------------------------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `audioInfo`                    | String          | Returns the audio pre-encoding info for the event type `onAudioPreEncodeProgress` (available since 24.0).                                                                                                                                                                                                          |
| `audioProgress`                | Float           | Returns the audio pre-encoding progress for the event type `onAudioPreEncodeProgress` (available since 24.0).                                                                                                                                                                                                      |
| `batchEncoderStatus`           | String          | Returns the status of the batch encoder, when the event was sent. Can be called for `onBatchEncoderStatusChanged` event, otherwise the status will be invalid. The values are:<ul><li>`"invalid"`</li><li>`"paused"`</li><li>`"running"`</li><li>`"stopped"`</li><li>`"stopping"`</li></ul>(available since 23.3). |
| `onAudioPreEncodeProgress`     | Constant string | Notify when the audio pre-encode progress changes (available since 24.0).                                                                                                                                                                                                                                          |
| `onBatchEncoderStatusChanged`  | Constant string | Notify when the batch encoder status has changed. Get the new status from the `batchEncoderStatus` property. (available since 23.3)                                                                                                                                                                                |
| `onEncodingItemProgressUpdate` | Constant string | Notify of the batch item encoding progress (available since 23.1).                                                                                                                                                                                                                                                 |
| `onItemEncodeCompleted`        | Constant string | Notify when the batch item has been encoded.                                                                                                                                                                                                                                                                       |
| `onItemEncodingStarted`        | Constant string | Notify when the batch item encoding started (available since 23.1).                                                                                                                                                                                                                                                |
| `outputFilePath`               | String          | Returns the path of the output file. Can be called for `onItemEncodingStarted` and `onItemEncodeComplete` events.                                                                                                                                                                                                  |
| `progress`                     | Float           | Returns the encoding progress between 0 and 1. Can be called for `onEncodingItemProgressUpdate` event.                                                                                                                                                                                                             |
| `result`                       | String          | Returns the encoding result `True` or `False`. Can be called for `onItemEncodeComplete` event.                                                                                                                                                                                                                     |
| `sourceFilePath`               | String          | Returns the path of the source file. Can be called for `onItemEncodingStarted` and `onItemEncodeComplete` events.                                                                                                                                                                                                  |

---

## Examples

```javascript
// Please use this event when you have multiple batch items in the queue (added manually or via a script as below)
// to ensure you receive all event types
var source_1 = "C:\\testdata\\testmedia1.mxf";
var source_2 = "C:\\testdata\\testmedia2.mxf";
var source_3 = "C:\\testdata\\testmedia3.mxf";

// //sources for mac
// var source_1 = "/Users/Shared/testdata/testmedia1.mxf"
// var source_2 = "/Users/Shared/testdata/testmedia2.mxf";
// var source_3 = "/Users/Shared/testdata/testmedia3.mxf";

var frontend = app.getFrontend();
if (frontend) {
  // listen for batch item added event
  frontend.addEventListener("onItemAddedToBatch", function (eventObj) {
    $.writeln("frontend.onItemAddedToBatch: success");
  });

  var batchItemSuccess_1 = frontend.addItemToBatch(source_1);
  var batchItemSuccess_2 = frontend.addItemToBatch(source_2);
  var batchItemSuccess_3 = frontend.addItemToBatch(source_3);
  if (batchItemSuccess_1 && batchItemSuccess_2 && batchItemSuccess_3) {
    $.writeln(
      "Batch item added successfully for the source files ",
      source_1 + " , " + source_2 + " , " + source_3
    );

    encoderHost = app.getEncoderHost();
    if (encoderHost) {
      // listen to the item encoding started event (available since 23.1.)
      encoderHost.addEventListener(
        "onItemEncodingStarted",
        function (eventObj) {
          $.writeln(
            "onItemEncodingStarted: Source File Path: " +
              eventObj.sourceFilePath
          );
          $.writeln(
            "onItemEncodingStarted: Output File Path: " +
              eventObj.outputFilePath
          );
        }
      );

      /* for earlier versions (23.0. or older) there's an additional step necessary to listen to the onItemEncodingStarted event
        var exporter = app.getExporter();
        if (exporter) {
            exporter.addEventListener(
                "onItemEncodingStarted",
                function (eventObj) {
                $.writeln("onItemEncodingStarted");
                }
            );
        }
      */

      // listen to the item encoding progress event (available since 23.1.)
      encoderHost.addEventListener(
        "onEncodingItemProgressUpdate",
        function (eventObj) {
          $.writeln(
            "onEncodingItemProgessUpdate: Encoding Progress: " +
              eventObj.progress
          );
        }
      );

      // listen to the audio pre-encoding progress event (available since 24.0.)
      encoderHost.addEventListener(
        "onAudioPreEncodeProgress",
        function (eventObj) {
          $.writeln("Audio pre-encoding info: " + eventObj.audioInfo);
          $.writeln("Audio pre-encoding progress: " + eventObj.audioProgress);
        },
        false
      );

      /* for earlier versions (23.0. or older) there's an additional step necessary to listen to the onEncodingItemProgressUpdated event
        var exporter = app.getExporter();
        if (exporter) {
            exporter.addEventListener(
                "onEncodingItemProgressUpdated",
                function (eventObj) {
                $.writeln("onEncodingItemProgessUpdated: Encoding Progress: " + eventObj.progress);
                }
            );
        }
      */

      // listen to the item encoding complete event
      encoderHost.addEventListener("onItemEncodeComplete", function (eventObj) {
        $.writeln("onItemEncodeComplete: Result: " + eventObj.result);
        $.writeln(
          "onItemEncodeComplete: Source File Path: " + eventObj.sourceFilePath
        );
        $.writeln(
          "onItemEncodeComplete: Output File Path: " + eventObj.outputFilePath
        );
      });

      encoderHost.runBatch();
    } else {
      $.writeln("encoderHost not valid");
    }
  } else {
    $.writeln("batch item wasn't added successfully");
  }
} else {
  $.writeln("frontend not valid");
}
```
