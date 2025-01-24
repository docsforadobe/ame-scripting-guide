# ExporterScript Object

`app.getExporter()`

Contains several encoding methods.

---

## Attributes

### ExporterScript.elapsedMilliseconds

`app.getExporter().elapsedMilliseconds`

#### Description

Returns the encode time in milliseconds.

#### Type

Float

#### Example

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
  exporter.exportItem(source, destination, preset);
  exporter.addEventListener("onEncodeComplete", function (eventObj) {
    // We can get the encoding time from the event or from the exporter
    $.writeln(
      "Encode Complete Time (in milli seconds): " + eventObj.encodeCompleteTime
    );

    var encodeCompleteTimeMilliseconds = exporter.elapsedMilliseconds;
    $.writeln(
      "Encode Complete Time alt (in milli seconds): " +
        encodeCompleteTimeMilliseconds
    );
  });
}
```

---

### ExporterScript.encodeID

`app.getExporter().encodeID`

#### Description

Returns the current encode item ID as string.

#### Type

String

#### Example

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
  var encoderWrapper = exporter.exportItem(source, destination, preset);
  var encodeID = exporter.encodeID;
  $.writeln("Encode ID: " + encodeID);
}
```

---

## Methods

### ExporterScript.exportGroup()

`app.getExporter().exportGroup(sourcePath, outputPath, presetsPath, matchSource)`

#### Description

Export the source with the provided list of presets.

#### Parameters

|   Parameter   |  Type   |                                                         Description                                                         |
| ------------- | ------- | --------------------------------------------------------------------------------------------------------------------------- |
| `sourcePath`  | String  | Media path (Premiere Pro projects aren't supported).                                                                        |
| `outputPath`  | String  | If `outputPath` is empty, then the output file location will be generated based on the source location.                     |
| `presetsPath` | String  | Multiple preset paths can be provided separated via a <code>&#124;</code> (e.g. <code>"path1&#124;path2&#124;path3"</code>) |
| `matchSource` | Boolean | Optional. Default value is `false`.                                                                                         |

#### Returns

Boolean; `true` in case of success.

#### Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset_1 = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var preset_2 = "C:\\testdata\\XDCAMHD 50 PAL 25p.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset_1 = "/Users/Shared/testdata/XDCAMHD 50 PAL 50i.epr";
// var preset_2 = "/Users/Shared/testdata/XDCAMHD 50 PAL 25p.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var matchSourceSettings = false; // optional
var presets = preset_1 + "|" + preset_2;

var exporter = app.getExporter();
if (exporter) {
  exporter.addEventListener(
    "onEncodeComplete",
    function (eventObj) {
      // We should arrive here two times (for every preset we have one batch item)
      $.writeln(
        "Encode Complete Status (always true): " + eventObj.encodeCompleteStatus
      );
      // We encode both batch items in parallel and so we don't really get the exact time for each batch item
      // When we arrive here the second time we get the total encoding time for both batch items (the first
      // could be ignored)
      $.writeln(
        "Encode Complete Time (in milliseconds): " + eventObj.encodeCompleteTime
      );
    },
    false
  );

  exporter.addEventListener(
    "onError",
    function (eventObj) {
      $.writeln("Error while encoding");
    },
    false
  );

  exporter.addEventListener(
    "onBatchItemStatusChanged",
    function (eventObj) {
      $.writeln("Batch group index: " + eventObj.groupIndex);
      $.writeln("Batch item index: " + eventObj.itemIndex);
      /*
        Possible status values:
        kBatchItemStatus_Waiting = 0,
        kBatchItemStatus_Done,
        kBatchItemStatus_Failed,
        kBatchItemStatus_Skipped,
        kBatchItemStatus_Encoding,
        kBatchItemStatus_Paused,
        kBatchItemStatus_Stopped,
        kBatchItemStatus_Any,
        kBatchItemStatus_AutoStart,
        kBatchItemStatus_Done_Warning,
        kBatchItemStatus_WatchFolderWaiting
        */
      $.writeln("Batch item status: " + eventObj.status);
    },
    false
  );

  exporter.addEventListener(
    "onItemEncodingStarted",
    function (eventObj) {
      $.writeln("Encoding started for batch item.");
    },
    false
  );

  exporter.addEventListener(
    "onMediaInfoCreated",
    function (eventObj) {
      $.writeln("Media info created");
    },
    false
  );

  exporter.addEventListener(
    "onPostProcessListInitialized",
    function (eventObj) {
      $.writeln("Post process list has been initialized.");
    },
    false
  );

  var encodingPreperationSuccess = exporter.exportGroup(
    source,
    destination,
    presets,
    matchSourceSettings
  );
  // Without all optional arguments:
  // var encodingPreperationSuccess = exporter.exportGroup(source, destination, presets);

  $.writeln(
    "Encoding preparations were successful: " + encodingPreperationSuccess
  );
}
```

---

### ExporterScript.exportItem()

`app.getExporter().exportItem(sourcePath, outputPath, presetsPath, matchSource, writeFramesToDisk)`

#### Description

Export the source with the provided preset.

#### Parameters

|      Parameter      |  Type   |                                                                                         Description                                                                                          |
| ------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sourcePath`        | String  | Media path or Premiere Pro project path (In case of a Premiere Pro project, the last sequence will be used).                                                                                 |
| `outputPath`        | String  | If `outputPath` is empty, then the output file location will be generated based on the source location.                                                                                      |
| `presetsPath`       | String  | Multiple preset paths can be provided separated via a <code>&#124;</code> (e.g. <code>"path1&#124;path2&#124;path3"</code>)                                                                  |
| `matchSource`       | Boolean | Optional. Default value is `false`.                                                                                                                                                          |
| `writeFramesToDisk` | Boolean | Optional. Default value is `false`. `true` writes five frames at 0%, 25%, 50%, 75% and 100% of the full duration.<br />Known issue: Currently it only works with parallel encoding disabled. |

#### Returns

[EncoderWrapper object](./EncoderWrapper.md)

#### Example

```javascript
// Supported: PR projects (last sequence will be used)
// var source = "C:\\testdata\\prProjectTest.prproj;
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\testdata\\outputFolder";
var matchSourceSettings = false; // optional
var writeFramesToDisk = false; // optional

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/XDCAMHD 50 PAL 50i.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var exporter = app.getExporter();

if (exporter) {
  // listen to events dispatched by the exporter:
  exporter.addEventListener(
    "onEncodeComplete",
    function (eventObj) {
      $.writeln(
        "Encode Complete Status (always true): " + eventObj.encodeCompleteStatus
      ); // Complete status always true
      $.writeln(
        "Encode Complete Time (in milliseconds): " + eventObj.encodeCompleteTime
      );
    },
    false
  );

  exporter.addEventListener(
    "onError",
    function (eventObj) {
      $.writeln("Error while encoding");
    },
    false
  );

  exporter.addEventListener(
    "onBatchItemStatusChanged",
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

  exporter.addEventListener(
    "onEncodingItemProgressUpdated",
    function (eventObj) {
      $.writeln("Encoding progress for batch item: " + eventObj.progress);
    },
    false
  );

  // listen to the audio pre-encoding progress event (available since 24.0.)
  exporter.addEventListener(
    "onAudioPreEncodeProgress",
    function (eventObj) {
      $.writeln("Audio pre-encoding info: " + eventObj.audioInfo);
      $.writeln("Audio pre-encoding progress: " + eventObj.audioProgress);
    },
    false
  );

  exporter.addEventListener(
    "onItemEncodingStarted",
    function (eventObj) {
      $.writeln("Encoding started for batch item.");
    },
    false
  );

  exporter.addEventListener(
    "onMediaInfoCreated",
    function (eventObj) {
      $.writeln("Media info created");
    },
    false
  );

  exporter.addEventListener(
    "onPostProcessListInitialized",
    function (eventObj) {
      $.writeln("Post process list has been initialized.");
    },
    false
  );

  var encoderWrapper = exporter.exportItem(
    source,
    destination,
    preset,
    matchSourceSettings,
    writeFramesToDisk
  );
  // Without all optional arguments:
  // var encoderWrapper  = exporter.exportItem(source, destination, preset);

  if (encoderWrapper) {
    encoderWrapper.addEventListener(
      "onEncodeFinished",
      function (eventObj) {
        $.writeln("Encoding result: " + eventObj.result);
      },
      false
    );

    encoderWrapper.addEventListener(
      "onEncodeProgress",
      function (eventObj) {
        $.writeln("Encoding progress: " + eventObj.result);
      },
      false
    );
  }
}
```

---

### ExporterScript.exportSequence()

`app.getExporter().exportSequence(projectPath, outputPath, presetsPath, matchSource, writeFramesToDisk, leadingFramesToTrim, trailingFramesToTrim, sequenceName)`

#### Description

Export the Premiere Pro sequence with the provided preset.

#### Parameters

|       Parameter        |  Type   |                                                                                         Description                                                                                          |
| ---------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `projectPath`          | String  | Premiere Pro project path.                                                                                                                                                                   |
| `outputPath`           | String  | If `outputPath` is empty, then the output file location will be generated based on the source location.                                                                                      |
| `presetsPath`          | String  | Multiple preset paths can be provided separated via a <code>&#124;</code> (e.g. <code>"path1&#124;path2&#124;path3"</code>)                                                                  |
| `matchSource`          | Boolean | Optional. Default value is `false`.                                                                                                                                                          |
| `writeFramesToDisk`    | Boolean | Optional. Default value is `false`. `true` writes five frames at 0%, 25%, 50%, 75% and 100% of the full duration.<br />Known issue: Currently it only works with parallel encoding disabled. |
| `leadingFramesToTrim`  | Integer | Optional. Default value is `0`.                                                                                                                                                              |
| `trailingFramesToTrim` | Integer | Optional. Default value is `0`.                                                                                                                                                              |
| `sequenceName`         | String  | Optional. If sequence name is empty, then we use the last sequence of the project.                                                                                                           |

#### Returns

Boolean; `true` in case of success.

#### Example

```javascript
var preset = "C:\\testdata\\XDCAMHD50PAL25p.epr";
var destination = "C:\\testdata\\Output";
var projectPath = "C:\\testdata\\prProjectTest.prproj";

// //sources for mac
// var preset = "/Users/Shared/testdata/XDCAMHD50PAL25p.epr";
// var destination = "/Users/Shared/testdata/Output";
// var projectPath = "/Users/Shared/testdata/prProjectTest.prproj";

var matchSource = false;
var writeFramesToDisk = false;
var leadingFramesToTrim = 0;
var trailingFramesToTrim = 0;
var sequenceName = "AME-Test-Sequence";

var exporter = app.getExporter();

if (exporter) {
  var encodingPreperationSuccess = exporter.exportSequence(
    projectPath,
    destination,
    preset,
    matchSource,
    writeFramesToDisk,
    leadingFramesToTrim,
    trailingFramesToTrim,
    sequenceName
  );

  $.writeln(
    "Encoding preparations were successful: " + encodingPreperationSuccess
  );

  // please see 'exportGroup' how to register events
}
```

---

### ExporterScript.getSourceMediaInfo()

`app.getExporter().getSourceMediaInfo(sourcePath)`

#### Description

Returns a [SourceMediaInfo](./SourceMediaInfo.md) object.

#### Parameters

|  Parameter   |  Type  |                                                 Description                                                 |
| ------------ | ------ | ----------------------------------------------------------------------------------------------------------- |
| `sourcePath` | String | Media path or Premiere Pro project path (In case of a Premiere Pro project the last sequence will be used). |

#### Returns

[SourceMediaInfo object](./SourceMediaInfo.md)

#### Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"

var exporter = app.getExporter();
if (exporter) {
  var sourceMediaInfo = exporter.getSourceMediaInfo(source);
  if (sourceMediaInfo) {
    $.writeln("Success");
  }
}
```

---

### ExporterScript.removeAllBatchItems()

`app.getExporter().removeAllBatchItems()`

#### Description

Remove all batch items from the queue.

#### Returns

Boolean; `true` in case of success.

#### Example

```javascript
// Preparation: Be sure there are some batch items in the queue. Otherwise create them via scripting APIs or directly in the UI
// since we need some batch item in the queue to verify the API removeAllBatchItems
var exporter = app.getExporter();
if (exporter) {
  var success = exporter.removeAllBatchItems();
  $.writeln("Remove all batch items was successful: " + success);
}
```

---

### ExporterScript.trimExportForSR()

`app.getExporter().trimExportForSR(sourcePath, outputPath, presetsPath, matchSource, writeFramesToDisk, leadingFramesToTrim, trailingFramesToTrim)`

#### Description

Smart render the source with the provided preset.

#### Parameters

|       Parameter        |  Type   |                                                                                         Description                                                                                          |
| ---------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sourcePath`           | String  | Media path or Premiere Pro project path (In case of a Premiere Pro project the last sequence will be used).                                                                                  |
| `outputPath`           | String  | If outputPath is empty, then the output file location will be generated based on the source location.                                                                                        |
| `presetsPath`          | String  | Multiple preset paths can be provided separated via a <code>&#124;</code> (e.g. <code>"path1&#124;path2&#124;path3"</code>)                                                                  |
| `matchSource`          | Boolean | Optional. Default value is `false`.                                                                                                                                                          |
| `writeFramesToDisk`    | Boolean | Optional. Default value is `false`. `true` writes five frames at 0%, 25%, 50%, 75% and 100% of the full duration.<br />Known issue: Currently it only works with parallel encoding disabled. |
| `leadingFramesToTrim`  | Integer | Optional. Default value is `0`.                                                                                                                                                              |
| `trailingFramesToTrim` | Integer | Optional. Default value is `0`.                                                                                                                                                              |

#### Returns

Boolean; `true` in case of success.

#### Example

```javascript
var source = "C:\\testdata\\testmedia.mp4";
var preset = "C:\\testdata\\XDCAMHD50PAL25p.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia.mp4"
// var preset = "/Users/Shared/testdata/XDCAMHD50PAL25p.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var matchSource = false;
var writeFramesToDisk = false;
var leadingFramesToTrim = 10;
var trailingFramesToTrim = 700;

var exporter = app.getExporter();
if (exporter) {
  var encodingPreperationSuccess = exporter.trimExportForSR(
    source,
    destination,
    preset,
    matchSource,
    writeFramesToDisk,
    leadingFramesToTrim,
    trailingFramesToTrim
  );

  $.writeln(
    "Encoding preparations were successful: " + encodingPreperationSuccess
  );

  // please see 'exportGroup' how to register events
}
```

---

## Events

Note that in addition to accessing these events by string, they can also be accessed via the following const property:

- [`AMEExportEvent.onAudioPreEncodeProgress`](#onaudiopreencodeprogress)
- [`AMEExportEvent.onBatchItemStatusChanged`](#onbatchitemstatuschanged)
- [`AMEExportEvent.onEncodeComplete`](#onencodecomplete)
- [`AMEExportEvent.onEncodingItemProgressUpdated`](#onencodingitemprogressupdated)
- [`AMEExportEvent.onError`](#onerror)
- [`AMEExportEvent.onItemEncodingStarted`](#onitemencodingstarted)
- [`AMEExportEvent.onMediaInfoCreated`](#onmediainfocreated)
- [`AMEExportEvent.onPostProcessListInitialized`](#onpostprocesslistinitialized)

---

### onAudioPreEncodeProgress

`app.getExporter().addEventListener("onAudioPreEncodeProgress")`

!!! note
    This functionality was added in Media Encoder 24.0

#### Description

Notify when the audio pre-encode progress changes.

#### Properties

|    Property     |  Type  |               Description               |
| --------------- | ------ | --------------------------------------- |
| `audioInfo`     | String | Returns the audio pre-encoding info     |
| `audioProgress` | Float  | Returns the audio pre-encoding progress |

---

### onBatchItemStatusChanged

`app.getExporter().addEventListener("onBatchItemStatusChanged")`

#### Description

Notify when batch item status has been changed.

#### Properties

|   Property   |     Type     |                                                                                                                                        Description                                                                                                                                        |
| ------------ | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `groupIndex` | Unsigned int | Returns the batch group index.                                                                                                                                                                                                                                                            |
| `itemIndex`  | Unsigned int | Returns the batch item index.                                                                                                                                                                                                                                                             |
| `status`     | Unsigned int | Returns the batch item status, one of: <ul><li>0 – Waiting</li><li>1 – Done</li><li>2 – Failed</li><li>3 – Skipped</li><li>4 – Encoding</li><li>5 – Paused</li><li>6 – Stopped</li><li>7 – Any</li><li>8 – AutoStart</li><li>9 – Done Warning</li><li>10 – Watch Folder Waiting</li></ul> |

#### Examples

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

##### status Example

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

---

### onEncodeComplete

`app.getExporter().addEventListener("onEncodeComplete")`

#### Description

Notify when the batch item has been encoded.

#### Properties

|        Property        |  Type   |                            Description                             |
| ---------------------- | ------- | ------------------------------------------------------------------ |
| `encodeCompleteStatus` | Boolean | Returns `true` after encoding has been completed for a batch item. |
| `encodeCompleteTime`   | Float   | Returns the encoding time in milliseconds.                         |

#### Examples

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

##### encodeCompleteStatus Example

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

##### encodeCompleteTime Example

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

### onEncodingItemProgressUpdated

`app.getExporter().addEventListener("onEncodingItemProgressUpdated")`

#### Description

Notify the encoding progress.

#### Properties

|  Property  | Type  |                               Description                                |
| ---------- | ----- | ------------------------------------------------------------------------ |
| `progress` | Float | Returns the batch item encoding progress value which is between 0 and 1. |

#### Examples

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

##### progress Example

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

### onError

`app.getExporter().addEventListener("onError")`

#### Description

Notify when there's an error while encoding the batch item.

#### Examples

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

### onItemEncodingStarted

`app.getExporter().addEventListener("onItemEncodingStarted")`

#### Description

Notify when the encoding of a batch item has started.

#### Examples

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

### onMediaInfoCreated

`app.getExporter().addEventListener("onMediaInfoCreated")`

#### Description

Notify when media info has been created.

#### Examples

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

### onPostProcessListInitialized

`app.getExporter().addEventListener("onPostProcessListInitialized")`

#### Description

Notify when the post process list is initialized.

#### Examples

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
