# AMEExportEvent

`AMEExportEvent`

Provides the following event types:

- `onMediaInfoCreated`
- `onBatchItemStatusChanged`
- `onItemEncodingStarted`
- `onAudioPreEncodeProgress`
- `onEncodingItemProgressUpdated`
- `onEncodeComplete`
- `onError`
- `onPostProcessListInitialized`

## Properties

|            Property             |      Type       |                                                                                                                                                        Description                                                                                                                                                         |
| ------------------------------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `audioInfo`                     | String          | Returns the audio pre-encoding info for the event type `onAudioPreEncodeProgress` (available since 24.0).                                                                                                                                                                                                                  |
| `audioProgress`                 | Float           | Returns the audio pre-encoding progress for the event type `onAudioPreEncodeProgress` (available since 24.0).                                                                                                                                                                                                              |
| `encodeCompleteStatus`          | Boolean         | Returns true after encoding has been completed for a batch item. Can be called for `onEncodeComplete` event.                                                                                                                                                                                                               |
| `encodeCompleteTime`            | Float           | Returns the encoding time in milliseconds. Can be called for `onEncodeComplete` event.                                                                                                                                                                                                                                     |
| `groupIndex`                    | Unsigned int    | Returns the batch group index. Can be called for `onBatchItemStatusChanged` event.                                                                                                                                                                                                                                         |
| `itemIndex`                     | Unsigned int    | Returns the batch item index. Can be called for `onBatchItemStatusChanged` event.                                                                                                                                                                                                                                          |
| `onAudioPreEncodeProgress`      | Constant string | Notify when the audio pre-encode progress changes (available since 24.0)                                                                                                                                                                                                                                                   |
| `onBatchItemStatusChanged`      | Constant string | Notify when batch item status has been changed. You can call the API's `groupIndex`, `itemIndex` and `status` for more info.                                                                                                                                                                                               |
| `onEncodeComplete`              | Constant string | Notify when the batch item has been encoded. You can call the API's `encodeCompleteStatus` and `encodeCompleteTime` for more info.                                                                                                                                                                                         |
| `onEncodingItemProgressUpdated` | Constant string | Notify the encoding progress.                                                                                                                                                                                                                                                                                              |
| `onError`                       | Constant string | Notify when there’s an error while encoding the batch item.                                                                                                                                                                                                                                                                |
| `onItemEncodingStarted`         | Constant string | Notify when the encoding of a batch item has started.                                                                                                                                                                                                                                                                      |
| `onMediaInfoCreated`            | Constant string | Notify when media info has been created.                                                                                                                                                                                                                                                                                   |
| `onPostProcessListInitialized`  | Constant string | Notify when the post process list is initialized.                                                                                                                                                                                                                                                                          |
| `progress`                      | Float           | Returns the batch item encoding progress value which is between 0 and 1. Can be called for `onEncodingItemProgressUpdated` event                                                                                                                                                                                           |
| `status`                        | Unsigned int    | Returns the batch item status. <ul><li>0: Waiting</li><li>1: Done</li><li>2: Failed</li><li>3: Skipped</li><li>4: Encoding</li><li>5: Paused</li><li>6: Stopped</li><li>7: Any</li><li>8: AutoStart</li><li>9: Done Warning</li><li>10 : Watch Folder Waiting</li></ul>Can be called for `onBatchItemStatusChanged` event. |

## Examples

### encodeCompleteStatus Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/XDCAMHD 50 PAL 50i.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var exporter = app.getExporter();
if (exporter) {
  exporter.addEventListener(
    "onEncodeComplete",
    function (eventObj) {
      $.writeln("Encode Complete Status: " + eventObj.encodeCompleteStatus);
    },
    false
  );

  // Alternatively you can access the correct name of that event via the following const property:
  var encodeCompleteEvent = AMEExportEvent.onEncodeComplete;
  exporter.addEventListener(
    encodeCompleteEvent,
    function (eventObj) {
      $.writeln(
        "Encode Complete Status (alt): " + eventObj.encodeCompleteStatus
      );
    },
    false
  );

  var encoderWrapper = exporter.exportItem(source, destination, preset);
}
```

---

### encodeCompleteTime Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/XDCAMHD 50 PAL 50i.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var exporter = app.getExporter();
if (exporter) {
  exporter.addEventListener(
    "onEncodeComplete",
    function (eventObj) {
      $.writeln(
        "Encode Complete Time in milli seconds: " + eventObj.encodeCompleteTime
      );
    },
    false
  );

  // Alternatively you can access the correct name of that event via the following const property:
  var encodeCompleteEvent = AMEExportEvent.onEncodeComplete;
  exporter.addEventListener(
    encodeCompleteEvent,
    function (eventObj) {
      $.writeln(
        "Encode Complete Time in milli seconds: (alt): " +
          eventObj.encodeCompleteTime
      );
    },
    false
  );

  var encoderWrapper = exporter.exportItem(source, destination, preset);
}
```

---

### onBatchItemStatusChanged Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/XDCAMHD 50 PAL 50i.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var batchItemStatusChangedEvent = AMEExportEvent.onBatchItemStatusChanged;
$.writeln(
  "Event name is identical with the const property API name ('onBatchItemStatusChanged'): " +
    batchItemStatusChangedEvent
);
var exporter = app.getExporter();

if (exporter) {
  exporter.addEventListener(
    batchItemStatusChangedEvent,
    function (eventObj) {
      $.writeln("Batch group index: " + eventObj.groupIndex);
      $.writeln("Batch item index: " + eventObj.itemIndex);
      //Possible status values:
      //kBatchItemStatus_Waiting = 0,
      //kBatchItemStatus_Done,
      //kBatchItemStatus_Failed,
      //kBatchItemStatus_Skipped,
      //kBatchItemStatus_Encoding,
      //kBatchItemStatus_Paused,
      //kBatchItemStatus_Stopped,
      //kBatchItemStatus_Any,
      //kBatchItemStatus_AutoStart,
      //kBatchItemStatus_Done_Warning,
      //kBatchItemStatus_WatchFolderWaiting
      $.writeln("Batch item status: " + eventObj.status);
    },
    false
  );

  // Alternatively you can listen to "onBatchItemStatusChanged"
  exporter.addEventListener(
    "onBatchItemStatusChanged",
    function (eventObj) {
      $.writeln("Batch group index (alt): " + eventObj.groupIndex);
      $.writeln("Batch item index (alt): " + eventObj.itemIndex);
      //Possible status values:
      //kBatchItemStatus_Waiting = 0,
      //kBatchItemStatus_Done,
      //kBatchItemStatus_Failed,
      //kBatchItemStatus_Skipped,
      //kBatchItemStatus_Encoding,
      //kBatchItemStatus_Paused,
      //kBatchItemStatus_Stopped,
      //kBatchItemStatus_Any,
      //kBatchItemStatus_AutoStart,
      //kBatchItemStatus_Done_Warning,
      //kBatchItemStatus_WatchFolderWaiting
      $.writeln("Batch item status (alt): " + eventObj.status);
    },
    false
  );

  var encoderWrapper = exporter.exportItem(source, destination, preset);
}
```

---

### onEncodeComplete Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/XDCAMHD 50 PAL 50i.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var encodeCompleteEvent = AMEExportEvent.onEncodeComplete;

$.writeln(
  "Event name is identical with the const property API name ('onEncodeComplete'): " +
    encodeCompleteEvent
);
var exporter = app.getExporter();

if (exporter) {
  exporter.addEventListener(
    encodeCompleteEvent,
    function (eventObj) {
      $.writeln("Encode Complete Status: " + eventObj.encodeCompleteStatus);
      $.writeln(
        "Encode Complete Time (in milli seconds): " +
          eventObj.encodeCompleteTime
      );
    },
    false
  );

  // Alternatively you can listen to "onEncodeComplete"
  exporter.addEventListener(
    "onEncodeComplete",
    function (eventObj) {
      $.writeln(
        "Encode Complete Status (alt): " + eventObj.encodeCompleteStatus
      );
      $.writeln(
        "Encode Complete Time in milli seconds (alt): " +
          eventObj.encodeCompleteTime
      );
    },
    false
  );

  var encoderWrapper = exporter.exportItem(source, destination, preset);
}
```

---

### onEncodingItemProgressUpdated Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/XDCAMHD 50 PAL 50i.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var encodingItemProgressUpdatedEvent =
  AMEExportEvent.onEncodingItemProgressUpdated;
$.writeln(
  "Event name is identical with the const property API name ('onEncodingItemProgressUpdated'): " +
    encodingItemProgressUpdatedEvent
);
var exporter = app.getExporter();

if (exporter) {
  exporter.addEventListener(
    encodingItemProgressUpdatedEvent,
    function (eventObj) {
      $.writeln("Encoding progress for batch item: " + eventObj.progress);
    },
    false
  );

  // Alternatively you can listen to "onEncodingItemProgressUpdated"
  exporter.addEventListener(
    "onEncodingItemProgressUpdated",
    function (eventObj) {
      $.writeln("Encoding progress for batch item (alt): " + eventObj.progress);
    },
    false
  );

  var encoderWrapper = exporter.exportItem(source, destination, preset);
}
```


---

### onError Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/XDCAMHD 50 PAL 50i.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var errorEvent = AMEExportEvent.onError;
$.writeln(
  "Event name is identical with the const property API name ('onError'): " +
    errorEvent
);

var exporter = app.getExporter();

if (exporter) {
  exporter.addEventListener(
    errorEvent,
    function (eventObj) {
      $.writeln("Error while encoding");
    },
    false
  );

  // Alternatively you can listen to "onError"
  exporter.addEventListener(
    "onError",
    function (eventObj) {
      $.writeln("Error while encoding (alt)");
    },
    false
  );

  var encoderWrapper = exporter.exportItem(source, destination, preset);
}
```

---

### onItemEncodingStarted Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/XDCAMHD 50 PAL 50i.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var itemEncodingStartedEvent = AMEExportEvent.onItemEncodingStarted;
$.writeln(
  "Event name is identical with the const property API name ('onItemEncodingStarted'): " +
    itemEncodingStartedEvent
);
var exporter = app.getExporter();

if (exporter) {
  exporter.addEventListener(
    itemEncodingStartedEvent,
    function (eventObj) {
      $.writeln("Encoding started for batch item.");
    },
    false
  );

  // Alternatively you can listen to "onItemEncodingStarted"
  exporter.addEventListener(
    "onItemEncodingStarted",
    function (eventObj) {
      $.writeln("Encoding started for batch item (alt).");
    },
    false
  );

  var encoderWrapper = exporter.exportItem(source, destination, preset);
}
```

---

### onMediaInfoCreated Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/XDCAMHD 50 PAL 50i.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var mediaInfoCreatedEvent = AMEExportEvent.onMediaInfoCreated;
$.writeln(
  "Event name is identical with the const property API name ('onMediaInfoCreated'): " +
    mediaInfoCreatedEvent
);
var exporter = app.getExporter();

if (exporter) {
  exporter.addEventListener(
    mediaInfoCreatedEvent,
    function (eventObj) {
      $.writeln("Media info created");
    },
    false
  );

  // Alternatively you can listen to "onMediaInfoCreated"
  exporter.addEventListener(
    "onMediaInfoCreated",
    function (eventObj) {
      $.writeln("Media info created (alt)");
    },
    false
  );

  var encoderWrapper = exporter.exportItem(source, destination, preset);
}
```

---

### onPostProcessListInitialized Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/XDCAMHD 50 PAL 50i.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var postProcessListInitializedEvent =
  AMEExportEvent.onPostProcessListInitialized;
$.writeln(
  "Event name is identical with the const property API name ('onPostProcessListInitialized'): " +
    postProcessListInitializedEvent
);
var exporter = app.getExporter();

if (exporter) {
  exporter.addEventListener(
    postProcessListInitializedEvent,
    function (eventObj) {
      $.writeln("Post process list has been initialized.");
    },
    false
  );

  // Alternatively you can listen to "onPostProcessListInitialized"
  exporter.addEventListener(
    "onPostProcessListInitialized",
    function (eventObj) {
      $.writeln("Post process list has been initialized (alt).");
    },
    false
  );

  var encoderWrapper = exporter.exportItem(source, destination, preset);
}
```

---

### progress Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/XDCAMHD 50 PAL 50i.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var exporter = app.getExporter();

if (exporter) {
  exporter.addEventListener(
    "onEncodingItemProgressUpdated",
    function (eventObj) {
      $.writeln("Encoding progress for batch item: " + eventObj.progress);
    },
    false
  );

  // Alternatively you can access the correct name of that event via the following const property:
  var encodingItemProgressUpdatedEvent =
    AMEExportEvent.onEncodingItemProgressUpdated;
  exporter.addEventListener(
    encodingItemProgressUpdatedEvent,
    function (eventObj) {
      $.writeln("Encoding progress for batch item (alt): " + eventObj.progress);
    },
    false
  );

  var encoderWrapper = exporter.exportItem(source, destination, preset);
}
```

---

### status Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/XDCAMHD 50 PAL 50i.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var exporter = app.getExporter();

if (exporter) {
  exporter.addEventListener(
    "onBatchItemStatusChanged",
    function (eventObj) {
      //Possible status values:
      // 0 : Waiting
      // 1 : Done
      // 2 : Failed
      // 3 : Skipped
      // 4 : Encoding
      // 5 : Paused
      // 6 : Stopped
      // 7 : Any
      // 8 : AutoStart
      // 9 : Done Warning
      // 10 : Watch Folder Waiting.
      $.writeln("Batch item status: " + eventObj.status);
    },
    false
  );

  // Alternatively you can access the correct name of that event via the following const property:
  var batchItemStatusChangedEvent = AMEExportEvent.onBatchItemStatusChanged;
  exporter.addEventListener(
    batchItemStatusChangedEvent,
    function (eventObj) {
      //Possible status values:
      // 0 : Waiting
      // 1 : Done
      // 2 : Failed
      // 3 : Skipped
      // 4 : Encoding
      // 5 : Paused
      // 6 : Stopped
      // 7 : Any
      // 8 : AutoStart
      // 9 : Done Warning
      // 10 : Watch Folder Waiting.
      $.writeln("Batch item status (alt): " + eventObj.status);
    },
    false
  );

  var encoderWrapper = exporter.exportItem(source, destination, preset);
}
```
