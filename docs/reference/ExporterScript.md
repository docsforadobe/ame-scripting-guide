# ExporterScript Object

`app.getExporter()`

Contains several encoding methods.

You can listen to different types of the AMEExportEvent:

- `onEncodeComplete`
- `onError`
- `onMediaInfoCreated`
- `onBatchItemStatusChanged`
- `onItemEncodingStarted`
- `onEncodingItemProgressUpdated`
- `onAudioPreEncodeProgress`
- `onPostProcessListInitialized`

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

[EncoderWrapper object](./todo.md)

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

`app.getExporter().getSourceMediaInfo(sourcePath): [SourceMediaInfo object](./todo.md)`

#### Description

Returns a [SourceMediaInfo](./todo.md) object.

#### Parameters

|  Parameter   |  Type  |                                                 Description                                                 |
| ------------ | ------ | ----------------------------------------------------------------------------------------------------------- |
| `sourcePath` | String | Media path or Premiere Pro project path (In case of a Premiere Pro project the last sequence will be used). |

#### Returns

[SourceMediaInfo object](./todo.md)

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
