# EncoderHostScript Object

`app.getEncoderHost()`

Provides several utility methods including batch commands to run,
pause or stop the batch.

---

## Methods

### EncoderHostScript.createEncoderForFormat()

`app.getEncoderHost().createEncoderForFormat(inFormatName)`

#### Description

Creates an [EncoderWrapper](./EncoderWrapper.md) for the requested format.

#### Parameters

|   Parameter    |  Type  |      Description       |
| -------------- | ------ | ---------------------- |
| `inFormatName` | String | The name of the format |

#### Returns

[EncoderWrapper object](./EncoderWrapper.md)

---

### EncoderHostScript.getBatchEncoderStatus()

`app.getEncoderHost().getBatchEncoderStatus()`

!!! note
    This functionality was added in Media Encoder 23.3

#### Description

Returns the current status of the batch encoder.

#### Returns

String, one of:

- `"invalid"`
- `"paused"`
- `"running"`
- `"stopped"`
- `"stopping"`

---

### EncoderHostScript.getCurrentBatchPreview()

`app.getEncoderHost().getCurrentBatchPreview(inOutputPath)`

#### Description

Writes out the current batch preview image (`tiff` format) to the given path.

#### Parameters

|   Parameter    |  Type  |              Description              |
| -------------- | ------ | ------------------------------------- |
| `inOutputPath` | String | The path to write the preview file to |

#### Returns

Boolean

---

### EncoderHostScript.getFormatList()

`app.getEncoderHost().getFormatList()`

#### Description

Returns a list of all available formats.

#### Returns

Array of strings

---

### EncoderHostScript.getSourceInfo()

`app.getEncoderHost().getSourceInfo(sourcePath)`

#### Description

Gets detailed info about a provided source.

#### Parameters

|  Parameter   |  Type  |              Description               |
| ------------ | ------ | -------------------------------------- |
| `sourcePath` | String | The media path to get source info from |

#### Returns

[SourceMediaInfo object](./SourceMediaInfo.md)

---

### EncoderHostScript.getSupportedImportFileTypes()

`app.getEncoderHost().getSupportedImportFileTypes()`

#### Description

Returns list of all available formats.

#### Returns

Array of strings

---

### EncoderHostScript.isBatchRunning()

`app.getEncoderHost().isBatchRunning()`

#### Description

Checks whether a batch job is running.

#### Returns

Boolean; `true` if a batch job is running.

---

### EncoderHostScript.pauseBatch()

`app.getEncoderHost().pauseBatch()`

#### Description

Pauses the batch.

#### Returns

Boolean; always returns `true`.

---

### EncoderHostScript.runBatch()

`app.getEncoderHost().runBatch()`

#### Description

Runs the batch

#### Returns

Boolean; always returns `true`.

---

### EncoderHostScript.stopBatch()

`app.getEncoderHost().stopBatch()`

#### Description

Stops the batch.

#### Returns

Boolean; always returns `true`.

---

## Examples

```javascript
var format = "H.264"; // e.g. H.264
var source = "C:\\testdata\\testmedia1.mxf";
var outputFile = "C:\\testdata\\outputFolder\\output.tiff";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia1.mxf"
// var outputFile = "/Users/Shared/testdata/outputFolder/output.tiff";

var encoderHost = app.getEncoderHost();

if (encoderHost) {
  encoderHost.addEventListener(
    "onBatchEncoderStatusChanged",
    function (eventObj) {
      $.writeln(
        "onBatchEncoderStatusChanged to status: " + eventObj.batchEncoderStatus
      );
    }
  );

  // API "getSourceInfo"
  var sourceMediaInfo = encoderHost.getSourceInfo(source);
  if (sourceMediaInfo) {
    // For 'sourceMediaInfo' you can now call properties of the 'SourceMediaInfo' script object, e.g.:
    // (See detailed info in the documentation of 'SourceMediaInfo')
    $.writeln(
      "Embedded description of the source: " + sourceMediaInfo.description
    );
  }

  // API "getFormatList"
  var formatList = encoderHost.getFormatList();
  $.writeln("formatList: " + formatList);

  // API "createEncoderForFormat"
  var encoderWrapper = encoderHost.createEncoderForFormat(format);
  if (encoderWrapper) {
    // For 'encoder' you can now call properties/methods of the 'EncoderWrapper" script object, e.g.:
    // (See detailed info in the documentation of 'EncoderWrapper')
    var frameRate = "25";
    encoderWrapper.setFrameRate(frameRate);
  }

  // API "isBatchRunning"
  var isBatchRunning = encoderHost.isBatchRunning();
  // With the current script the return value should be 'false' since no batch (job) is running.
  // After adding batch items (see FrontendScriptObject) and calling encoderHost.runBatch() this method returns 'true' as long as a job is running.
  $.writeln("isBatchRunning: " + isBatchRunning);

  // API "getBatchEncoderStatus"
  var batchStatus = encoderHost.getBatchEncoderStatus();
  // expected value is "stopped", because the batch had not been started.
  // The values are: invalid, paused, running, stopped, stopping
  $.writeln("batch status is: " + batchStatus);

  // API "runBatch" (always returns true and therefore it's not necessary to store the result)
  encoderHost.runBatch();

  // API "pauseBatch" (always returns true and therefore it's not necessary to store the result)
  encoderHost.pauseBatch();

  // API "stopBatch" (always returns true and therefore it's not necessary to store the result)
  encoderHost.stopBatch();

  // API "getCurrentBatchPreview"
  var result = encoderHost.getCurrentBatchPreview(outputFile);
  $.writeln("result: " + result);

  // API "getSupportedImportFileTypes"
  var supportedFileTypes = encoderHost.getSupportedImportFileTypes();
  $.writeln("supportedFileTypes: " + supportedFileTypes);
} else {
  $.writeln("encoderHost script object not defined");
}
```

---

## Events

Provides the following event types for items in the batch queue:

- [onItemEncodingStarted](#onitemencodingstarted)
- [onAudioPreEncodeProgress](#onaudiopreencodeprogress)
- [onEncodingItemProgressUpdate](#onencodingitemprogressupdate)
- [onItemEncodeComplete](#onitemencodecomplete)

For multiple batch items in the queue we recommend to use this event to ensure that the event types will be received for all batch items.

It provides the following event type for the whole batch queue:

- [onBatchEncoderStatusChanged](#onbatchencoderstatuschanged)

### Example

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

---

### onAudioPreEncodeProgress

`app.getEncoderHost().addEventListener("onAudioPreEncodeProgress")`

!!! note
    This functionality was added in Media Encoder 24.0

#### Description

Notify when the audio pre-encode progress changes.

#### Properties

|    Property     |  Type  |           Description           |
| --------------- | ------ | ------------------------------- |
| `audioInfo`     | String | The audio pre-encoding info     |
| `audioProgress` | Float  | The audio pre-encoding progress |

---

### onBatchEncoderStatusChanged

`app.getEncoderHost().addEventListener("onBatchEncoderStatusChanged")`

!!! note
    This functionality was added in Media Encoder 23.3

#### Description

Notify when the batch encoder status has changed. Get the new status from the `batchEncoderStatus` property.

#### Properties

|       Property       |  Type  |                                                                                          Description                                                                                           |
| -------------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `batchEncoderStatus` | String | Returns the status of the batch encoder, when the event was sent. The values are:<ul><li>`"invalid"`</li><li>`"paused"`</li><li>`"running"`</li><li>`"stopped"`</li><li>`"stopping"`</li></ul> |

---

### onEncodingItemProgressUpdate

`app.getEncoderHost().addEventListener("onEncodingItemProgressUpdate")`

!!! note
    This functionality was added in Media Encoder 23.1

#### Description

Notify of the batch item encoding progress.

#### Properties

|  Property  | Type  |                  Description                   |
| ---------- | ----- | ---------------------------------------------- |
| `progress` | Float | Returns the encoding progress between 0 and 1. |

---

### onItemEncodeComplete

`app.getEncoderHost().addEventListener("onItemEncodeComplete")`

#### Description

Notify when the batch item has been encoded.

#### Properties

|     Property     |  Type  |                   Description                   |
| ---------------- | ------ | ----------------------------------------------- |
| `outputFilePath` | String | Returns the path of the output file.            |
| `result`         | String | Returns the encoding result, `True` or `False`. |
| `sourceFilePath` | String | Returns the path of the source file.            |

---

### onItemEncodingStarted

`app.getEncoderHost().addEventListener("onItemEncodingStarted")`

!!! note
    This functionality was added in Media Encoder 23.1

#### Description

Notify when the batch item encoding started.

#### Properties

|     Property     |  Type  |             Description              |
| ---------------- | ------ | ------------------------------------ |
| `outputFilePath` | String | Returns the path of the output file. |
| `sourceFilePath` | String | Returns the path of the source file. |
