# Scripting API Reference




## EncoderWrapper

**Queue item object to set encode properties**

<a id="properties-5"></a>

### Properties

- `outputFiles: array of strings` : Gets the list of files the
  encode generated
- `outputHeight: float` : Gets the height of the encoded output
  file
- `outputWidth: float` : Gets the width of the encoded output
  file

<a id="methods-2"></a>

### Methods

- `SetIncludeSourceXMP(includeSourceXMP: boolean): boolean` : Toggle
  the inclusion of source XMP [boolean] input value required
- `getEncodeProgress(): integer` : Returns the encode progress as
  percentage
- `getEncodeTime(): float` : Return the encode time in
  milliseconds
- `getLogOutput(): string` : Returns the log output including
  possible warnings and errors (available since 23.2.).
- `getMissingAssets(includeSource: boolean, includeOutput: boolean): array of strings` : Returns
  a list of missing assets
  - `includeSource`: Get missing asset list from the source group if
    requested
- `getPresetList(): array of strings` : Returns the presets
  available for the assigned format
- `loadFormat(format: string): boolean` : Changes the format for the
  batch item
  - `format`: E.g. `"H.264"` Loads all presets available for the
    assigned format
- `loadPreset(presetPath: string): boolean` : Loads and assigns the
  preset to the batch item
- `setCropOffsets(left: unsigned int, top: unsigned int, right: unsigned int, bottom: unsigned int): boolean` : Sets
  the crop offsets
- `setCropState(cropState: boolean): boolean` : Sets the crop state
  [boolean] input value required
- `setCropType(cropType: unsigned int): boolean` : Sets the scale
  type
  - `cropType`: 0 ScaleToFit, 1 ScaleToFitBeforeCrop, 2
    SetAsOutputSize, 3 ScaleToFill, 4 ScaleToFillBeforeCrop, 5
    StretchToFill, 6 StretchToFillBeforeCrop
- `setCuePointData(inCuePointsFilePath: string): boolean` : Sets the
  cue point data
- `setFrameRate(framerate: string): boolean` : Sets the frame rate
  for the batch item
  - `framerate`: E.g. `"24"` as string
- `setIncludeSourceCuePoints(includeSourceCuePoints: boolean): boolean` : Toggle
  the inclusion of cue points [boolean] input value required
- `setOutputFrameSize(width: unsigned int, height: unsigned int): boolean` : Sets
  the output frame size
- `setRotation(rotationValue: float): boolean` : Sets the rotation
  (in a 360 degree system)
  - `rotationValue`: E.g. 0.0 - 360.0
- `setScaleType(scaleType: unsigned int): boolean` : Sets the scale
  type
  - `scaleType`: 0 ScaleToFit, 1 ScaleToFitBeforeCrop, 2
    SetAsOutputSize, 3 ScaleToFill, 4 ScaleToFillBeforeCrop, 5
    StretchToFill, 6 StretchToFillBeforeCrop
- `setTimeInterpolationType(interpolationType: unsigned int): boolean` : Set
  the time interpolation type
  - `interpolationType`: 0 FrameSampling, 1 FrameBlending, 2
    OpticalFlow
- `setUseFrameBlending(useFrameBlending: boolean): boolean` : Toggle
  the use of frame blending [boolean] input value required
- `setUseMaximumRenderQuality(useMaximumRenderQuality: boolean): boolean` : Toggle
  the use of maximum render quality [boolean] input value required
- `setUsePreviewFiles(usePreviewFiles: boolean): boolean` : Toggle the
  use of previews files. [boolean] input value required
- `setWorkArea(workAreaType: unsigned int, startTime: float, endTime: float): boolean` : Sets
  the work area type, start and end time for the batch item
  - `workAreaType`: 0 Entire, 1 InToOut, 2 WorkArea, 3 Custom, 4
    UseDefault
- `setWorkAreaInTicks(workAreaType: unsigned int, startTime: string, endTime: string): boolean` : Sets
  the work area type, start and end time in ticks for the batch item
  (available since 23.3)
  - `workAreaType`: 0 Entire, 1 InToOut, 2 WorkArea, 3 Custom, 4
    UseDefault
- `setXMPData(templateXMPFilePath: string): boolean` : Sets XMP data
  to given template

<a id="code-samples-6"></a>

### Examples

<details>

<summary>Example (click to expand):</summary>
```javascript
var format = "";
var source = "C:\\testdata\\testmedia4.mp4";
var preset = "C:\\testdata\\HighQuality720HD.epr";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia4.mp4"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";

var frontend = app.getFrontend();
if (frontend) {
  // Either format or preset can be empty, output is optional
  var encoderWrapper = frontend.addFileToBatch(source, format, preset);

  if (encoderWrapper) {
    $.writeln(
      "Frontend script engine added the source file using addFileToBatch-",
      source,
      " successfully"
    );

    $.writeln("width :", encoderWrapper.outputWidth);
    $.writeln("height:", encoderWrapper.outputHeight);
    $.writeln("outputFiles:", encoderWrapper.outputFiles);

    //input value is string please use e.g. "25"
    encoderWrapper.setFrameRate("25");

    //int, 0-Entire, 1-InToOut, 2-WorkArea, 3-Custom, 4:UseDefault
    encoderWrapper.setWorkArea(2, 0.0, 1.0);

    var usePreviewFiles = true;
    encoderWrapper.setUsePreviewFiles(usePreviewFiles);

    var useMaximumRenderQuality = true;
    encoderWrapper.setUseMaximumRenderQuality(useMaximumRenderQuality);

    var useFrameBlending = true;
    encoderWrapper.setUseFrameBlending(useFrameBlending);

    // int-0-FrameSampling, 1-FrameBlending, 2-OpticalFlow
    encoderWrapper.setTimeInterpolationType(1);

    // be aware that this method first letter is upper case
    var includeSourceXMP = true;
    encoderWrapper.SetIncludeSourceXMP(includeSourceXMP);

    var includeSourceCuePoints = false;
    encoderWrapper.setIncludeSourceCuePoints(includeSourceCuePoints);

    var cropState = true;
    encoderWrapper.setCropState(cropState);

    //int, 0-ScaleToFit, 1-ScaleToFitBeforeCrop, 2-SetAsOutputSize, 3-ScaleToFill, 4-ScaleToFillBeforeCrop, 5-StretchToFill, 6-StretchToFillBeforeCrop",
    encoderWrapper.setCropType(4);

    //int, 0-ScaleToFit, 1-ScaleToFitBeforeCrop, 2-SetAsOutputSize, 3-ScaleToFill, 4-ScaleToFillBeforeCrop, 5-StretchToFill, 6-StretchToFillBeforeCrop",
    encoderWrapper.setScaleType(4);

    // rotate clockwise, input values will be transformed into [0 - 360], so -90 is equal to 270
    encoderWrapper.setRotation(180);

    //left, top, right, bottom
    encoderWrapper.setCropOffsets(10, 20, 10, 20);

    //width and height
    encoderWrapper.setOutputFrameSize(1200, 800);

    // default is off - deprecated
    //encoderWrapper.setCuePointData();

    var encoderHostWrapper = app.getEncoderHost();
    if (encoderHostWrapper) {
      encoderHostWrapper.runBatch();
    }
  } else {
    $.writeln("encoderWrapper is not valid");
  }
} else {
  $.writeln("frontend obj is not valid");
}
```

</details><br><details>

<summary>getLogOutput Example (click to expand):</summary>
```javascript
var format = "H.264";
var source = "C:\\testdata\\testmedia4.mp4";
var preset = "C:\\testdata\\HighQuality1080_HD.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia4.mp4"
// var preset = "/Users/Shared/testdata/HighQuality1080_HD.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var frontend = app.getFrontend();
if (frontend) {
  /**
   * getLogOutPut() returns a string in JSON format containing the possible errors and warnings as well as the summary of the batch item
   * that is added to the queue.
   *
   * The getLogOutput() method is implemented in the EncoderWrapperScriptObject.
   * You can use getLogOutput() method when you have used one of these following methods:
   *
   * FrontEndScriptObject:
   * - addFileToBatch()
   * - addDLToBatch()
   * - addTeamProjectsToBatch()
   * - stitchFiles()
   * In Addition it is possible to get the batch item status with
   * encoderWrapper.addListener("onStatusChanged"){...} Here you will get "Done!", "Failed!", "Stopped!"
   *
   * ExportScriptObject:
   * - export()
   * - getSourceMediaInfo()
   * In Addition it is possible to get the batch item status with
   * exporter.addListener("OnBatchItemStatusChanged"){...} Here you will get integer values see ExportScriptObject for the details
   *
   * EncoderHostWrapper:
   * - createEncoderFormat()
   *
   * Output format is
   *    {
   *        "time": "2023-01-16T12:18:36.617946",
   *        "error": "",
   *        "summary": []
   *    }
   */

  var encoderWrapper = frontend.addFileToBatch(
    source,
    format,
    preset,
    destination
  );
  if (encoderWrapper) {
    $.writeln("Batch item is successfully added to the queue: ", source);

    encoderWrapper.addEventListener("onEncodeFinished", function (eventObj) {
      // return the log output in JSON Format
      $.writeln(encoderWrapper.getLogOutput());
    });

    // get encoder host to run batch
    var encoderHost = app.getEncoderHost();
    if (encoderHost) {
      encoderHost.runBatch();
    } else {
      $.writeln("EncoderHostScriptObject is not valid");
    }
  } else {
    $.writeln(
      "EncoderWrapperScriptObject is not valid - batch item wasn't added successfully"
    );
  }
} else {
  $.writeln("FrontendScriptObject is not valid");
}
```

</details><br><details>

<summary>getPresetList Example (click to expand):</summary>
```javascript
var source = "C:\\testdata\\testmedia4.mp4";
var preset = "C:\\testdata\\HighQuality720HD.epr";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia4.mp4"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";

var format = "";
var frontend = app.getFrontend();
if (frontend) {
  var encoderWrapper = frontend.addFileToBatch(source, format, preset);

  if (encoderWrapper) {
    $.writeln(source, " has been added successfully");

    /**if you set the format parameter but no presetfilepath then you will
     * get all related presets to this specific format.
     *
     * If you set the presetfilepath but no format, then the
     * format will be set automatically that matches the current preset */

    var presetList = encoderWrapper.getPresetList();
    for (var index = 0; index < presetList.length; index++) {
      $.writeln(presetList[index]);
    }
  } else {
    $.writeln("encoderWrapper object is not valid");
  }
} else {
  $.writeln("Frontend object is not valid");
}
```

</details><br><details>

<summary>loadFormat Example (click to expand):</summary>
```javascript
var format = "";
var source = "C:\\testdata\\testmedia4.mp4";
var preset = "C:\\testdata\\HighQuality720HD.epr";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia4.mp4"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";

var frontend = app.getFrontend();
if (frontend) {
  var encoderWrapper = frontend.addFileToBatch(source, format, preset);
  if (encoderWrapper) {
    encoderWrapper.loadFormat("MP3");
  } else {
    $.writeln("EncoderWrapper object is not valid");
  }
} else {
  $.writeln("Frontend object is not valid");
}
```

</details><br><details>

<summary>loadPreset Example (click to expand):</summary>
```javascript
var format = "";
var source = "C:\\testdata\\testmedia4.mp4";
var preset = "C:\\testdata\\HighQuality720HD.epr";

var differentPreset = "C:\\testdata\\High Quality 1080 HD.epr";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia4.mp4"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";
// var differentPreset = "/Users/Shared/testdata/High Quality 1080 HD.epr";

var frontend = app.getFrontend();
if (frontend) {
  // Either format name or presetPath can be empty, output filepath is optional
  var encoderWrapper = frontend.addFileToBatch(source, format, preset);
  if (encoderWrapper) {
    encoderWrapper.loadPreset(differentPreset);
  } else {
    $.writeln("EncoderWrapper object is not valid");
  }
} else {
  $.writeln("Frontend object is not valid");
}
```

</details><br><details>

<summary>setWorkAreaInTicks Example (click to expand):</summary>
```javascript
var format = "H.264";

var source = "C:\\testdata\\testmedia4.mp4";
var preset = "C:\\testdata\\HD 720p.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia4.mp4"
// var preset = "/Users/Shared/testdata/HD 720p.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

// The value of ticksPerSecond is predefined in premiere pro and ame.
// For more information please have a look into https://ppro-scripting.docsforadobe.dev/other/time.html
var ticksPerSecond = 254016000000;
var startTimeInTicks = 20 * ticksPerSecond;
var timeToAddInTicks = 30 * ticksPerSecond;

var startTimeinTicksStr = String(startTimeInTicks);
var endTimeInTicksStr = String(timeToAddInTicks);

var frontend = app.getFrontend();
if (frontend) {
  var encoderWrapper = frontend.addFileToBatch(
    source,
    format,
    preset,
    destination
  );
  if (encoderWrapper) {
    $.writeln("workarea start time: ", startTimeinTicksStr);
    $.writeln("workarea end time: ", endTimeInTicksStr);
    encoderWrapper.setWorkAreaInTicks(
      2,
      startTimeinTicksStr,
      endTimeInTicksStr
    );
  } else {
    $.writeln("encoderWrapper is not valid");
  }
  var encoderHost = app.getEncoderHost();
  if (encoderHost) {
    encoderHost.runBatch();
  } else {
    $.writeln("encoderHost is not valid");
  }
} else {
  $.writeln("frontend is not valid");
}
```

</details><br>



</details><br>

## ExporterScriptObject

**Contains several encoding methods. You can listen to different types
of the AMEExportEvent: onEncodeComplete, onError, onMediaInfoCreated,
onBatchItemStatusChanged, onItemEncodingStarted,
onEncodingItemProgressUpdated, onAudioPreEncodeProgress,
onPostProcessListInitialized**

<a id="properties-7"></a>

### Properties

- `elapsedMilliseconds: float` : Returns the encode time in
  milliseconds.
- `encodeID: string` : Returns the current encode item ID as
  string.

<a id="methods-3"></a>

### Methods

- `exportGroup(sourcePath: string, outputPath: string, presetsPath: string, matchSource: boolean = false): boolean` : Export the source with the provided list of presets. Returns `true` in case of success.
  - `sourcePath`: Media path (Premiere Pro projects aren't supported).
  - `outputPath`: If `outputPath` is empty, then the output file location will be generated based on the source location.
  - `presetsPath`: Multiple preset paths can be provided separated via a | (e.g. `"path1|path2|path3"`)
  - `matchSource`: Optional. Default value is false
- `exportItem(sourcePath: string, outputPath: string, presetPath: string, matchSource: boolean = false, writeFramesToDisk: boolean = false): scripting object` : Export the source with the provided preset. Returns an `EncoderWrapper` object.
  - `sourcePath`: Media path or Premiere Pro project path (In case of a Premiere Pro project the last sequence will be used).
  - `outputPath`: If `outputPath` is empty, then the output file location will be generated based on the source location.
  - `matchSource`: Optional. Default value is `false`
  - `writeFramesToDisk`: Optional. Default value is `false`. `true` writes five frames at 0%, 25%, 50%, 75% and 100% of the full duration.<br />Known issue: Currently it only works with parallel encoding disabled.
- `exportSequence(projectPath: string, outputPath: string, presetPath: string, matchSource: boolean = false, writeFramesToDisk: boolean = false, leadingFramesToTrim: int = 0, trailingFramesToTrim: int = 0, sequenceName: string = ""): boolean` : Export the Premiere Pro sequence with the provided preset. Returns `true` in case of success.
  - `projectPath`: Premiere Pro project path.
  - `outputPath`: If `outputPath` is empty, then the output file location will be generated based on the source location.
  - `matchSource`: Optional. Default value is `false`.
  - `writeFramesToDisk`: Optional. Default value is `false`. `true` writes five frames at 0%, 25%, 50%, 75% and 100% of the full duration.<br />Known issue: Currently it only works with parallel encoding disabled.
  - `leadingFramesToTrim`: Optional. Default value is `0`.
  - `trailingFramesToTrim`: Optional. Default value is `0`.
  - `sequenceName`: Optional. If sequence name is empty then we use the last sequence of the project.
- `getSourceMediaInfo(sourcePath: string): scripting object` : Returns a `SourceMediaInfo` object.
- `removeAllBatchItems(): boolean` : Remove all batch items from the queue. Returns `true` in case of success.
- `trimExportForSR(sourcePath: string, outputPath: string, presetPath: string, matchSource: boolean = false, writeFramesToDisk: boolean = false, leadingFramesToTrim: int = 0, trailingFramesToTrim: int = 0): boolean` : Smart
  render the source with the provided preset. Returns true in case of
  success.
  - `sourcePath`: Media path or Premiere Pro project path (In case
    of a Premiere Pro project the last sequence will be used).
  - `outputPath`: If outputPath is empty, then the output file
    location will be generated based on the source location.
  - `matchSource`: Optional. Default value is false.
  - `writeFramesToDisk`: Optional. Default value is false. True
    writes five frames at 0%, 25%, 50%, 75% and 100% of the full
    duration. Known issue: Currently it only works with parallel
    encoding disabled.
  - `leadingFramesToTrim`: Optional. Default value is 0.
  - `trailingFramesToTrim`: Optional. Default value is 0.

<a id="code-samples-8"></a>

### Examples

<details>

<summary>elapsedMilliseconds Example (click to expand):</summary>
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

</details><br><details>

<summary>encodeID Example (click to expand):</summary>
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

</details><br><details>

<summary>exportGroup Example (click to expand):</summary>
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

</details><br><details>

<summary>exportItem Example (click to expand):</summary>
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

</details><br><details>

<summary>exportSequence Example (click to expand):</summary>
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

</details><br><details>

<summary>getSourceMediaInfo Example (click to expand):</summary>
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

</details><br><details>

<summary>removeAllBatchItems Example (click to expand):</summary>
```javascript
// Preparation: Be sure there are some batch items in the queue. Otherwise create them via scripting APIs or directly in the UI
// since we need some batch item in the queue to verify the API removeAllBatchItems
var exporter = app.getExporter();
if (exporter) {
  var success = exporter.removeAllBatchItems();
  $.writeln("Remove all batch items was successful: " + success);
}
```

</details><br><details>

<summary>trimExportForSR Example (click to expand):</summary>
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

</details><br>

## FrontendScriptObject

**Scripting methods to the frontend**

<a id="methods-4"></a>

### Methods

- `addCompToBatch(compPath: string, presetPath: string = "", outputPath: string = ""): boolean` : Adds the first comp of an After Effects project resp. the first sequence of a Premiere Pro project to the batch.
  - `compPath`: Path to e.g. an After Effects project or Premiere Pro project. The first comp resp. sequence will be used.
  - `presetPath`: Optional. If `presetPath` is empty, then the default preset will be applied.
  - `outputPath`: Optional. If `outputPath` is empty, then the output file name will be generated based on the comp path.
- `addDLToBatch(projectPath: string, format: string, presetPath: string, guid: string, outputPath: string = ""): scripting object` : Adds
  e.g. an After Effects comp or Premiere Pro sequence to the batch and returns an `EncoderWrapper` object.
  - `projectPath`: E.g. Premiere Pro or After Effects project path.
  - `format`: E.g. `"H.264"`
  - `presetPath`: Either a preset or a format input must be present. If no preset is used then the default preset of the specified format will be applied.
  - `guid`: The unique id of e.g. a Premiere Pro sequence or After Effects composition.
  - `outputPath`: Optional. If `outputPath` is empty, then the output file name will be generated based on the project path.
- `addFileSequenceToBatch(containingFolder: string, imagePath: string, presetPath: string, outputPath: string = ""): boolean` : Adds an image sequence to the batch. The images will be sorted in alphabetical order.
  - `containingFolder`: The folder containing image files.
  - `imagePath`: All images from the containing folder with the same extension will be added to the output file.
  - `outputPath`: Optional. If `outputPath` is empty, then the output
    file name will be generated based on the containingFolder name
- `addFileToBatch(filePath: string, format: string, presetPath: string, outputPath: string = ""): scripting object` : Adds a file to the batch and returns an `EncoderWrapper` object.
  - `filePath`: File path of a media source.
  - `format`: E.g. `"H.264"`
  - `presetPath`: Either a preset or a format input must be present. If no preset is used then the default preset of the specified format will be applied.
  - `outputPath`: Optional. If `outputPath` is empty, then the output file name will be generated based on the file path.
- `addItemToBatch(sourcePath: string): boolean` : Adds a media source to the batch.
  - `sourcePath`: Path of the media source.
- `addTeamProjectsItemToBatch(projectsURL: string, format: string, presetPath: string, outputPath: string): scripting object` : Adds a team project item to the batch and returns an `EncoderWrapper` object.
  - `projectsURL`: Team Projects URL or Team Projects Snap. You can
    create a tp2snap file in PPro for a ProjectItem via the scripting
    API saveProjectSnapshot.
  - `format`: E.g. `"H.264"`
  - `presetPath`: Either a preset or a format input must be present. If no preset is used then the default preset of the specified format will be applied.
- `addXMLToBatch(xmlPath: string, presetPath: string, outputFolderPath: string = ""): boolean` : Adds
  Final Cut Pro xml to the batch.
  - `xmlPath`: Path to a Final Cut Pro xml file.
  - `outputFolderPath`: Optional. If outputFolderPath is empty, then the output file name will be generated based on the XML file path.
- `getDLItemsAtRoot(projectPath: string): array of strings` : Returns the list of GUIDs for objects (sequences/comps) at the top/root level.
  - `projectPath`: E.g. Premiere Pro or After Effects project path.
- `stitchFiles(mediaPaths: string, format: string, presetPath: string, outputPath: string): scripting object` : Adds a batch item for the given media and returns an `EncoderWrapper` object.
  - `mediaPaths`: Semicolon delimited list of media paths.
  - `format`: E.g. `"H.264"`
  - `presetPath`: Either a preset or a format input must be present. If no preset is used then the default preset of the specified format will be applied.
- `stopBatch(): boolean` : Stops the batch.

<a id="code-samples-9"></a>

### Examples

<details>

<summary>addCompToBatch Example (click to expand):</summary>
```javascript
var projectPath = "C:\\testdata\\aeCompTest.aep";
var preset = "C:\\testdata\\HighQuality720HD.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/aeCompTest.aep"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var frontend = app.getFrontend();
if (frontend) {
  // listen for batch item added event
  frontend.addEventListener("onItemAddedToBatch", function (eventObj) {
    $.writeln("frontend.onItemAddedToBatch: success");
  });

  var batchItemSuccess = frontend.addCompToBatch(
    projectPath,
    preset,
    destination
  );
  if (batchItemSuccess) {
    $.writeln(
      "Frontend script engine added the source file ",
      projectPath,
      " successfully "
    );

    // get encoderHost to be able to listen for the item complete event
    encoderHost = app.getEncoderHost();
    if (encoderHost) {
      encoderHost.addEventListener("onItemEncodeComplete", function (eventObj) {
        $.writeln("Result: " + eventObj.result);
        $.writeln("Source File Path: " + eventObj.sourceFilePath);
        $.writeln("Output File Path: " + eventObj.outputFilePath);
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

</details><br><details>

<summary>addDLToBatch Example (click to expand):</summary>
```javascript
// The projectPath can be a path to an AfterEffects, Premiere Pro or Character Animator project
var format = "H.264";
var projectPath = "C:\\testdata\\aeCompTest.aep";
var preset = "C:\\testdata\\HighQuality720HD.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var projectPath = "/Users/Shared/testdata/aeCompTest.aep"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var frontend = app.getFrontend();
if (frontend) {
  // first we need the guid of the e.g. ae comps or ppro sequences
  var result = frontend.getDLItemsAtRoot(projectPath);
  $.writeln(result.length + " comps / sequences found.");

  // import e.g. the first comp / sequence
  if (result.length > 0) {
    // listen for batch item added / creation failed event
    frontend.addEventListener("onItemAddedToBatch", function (eventObj) {
      $.writeln("frontend.onItemAddedToBatch: success");
    });

    frontend.addEventListener("onBatchItemCreationFailed", function (eventObj) {
      $.writeln("frontend.onBatchItemCreationFailed: failed");
      $.writeln("srcFilePath: " + eventObj.srcFilePath);
      $.writeln("error: " + eventObj.error);
    });

    var encoderWrapper = frontend.addDLToBatch(
      projectPath,
      format,
      preset,
      result[0],
      destination
    );

    if (encoderWrapper) {
      $.writeln(
        "Batch item added successfully for comp / sequence guid: ",
        result[0]
      );

      // listen for encode progress and encode finish events
      encoderWrapper.addEventListener("onEncodeProgress", function (eventObj) {
        $.writeln("Encoding progress for batch item: " + eventObj.result);
      });

      encoderWrapper.addEventListener("onEncodeFinished", function (eventObj) {
        $.writeln("Encoding result for batch item: " + eventObj.result);
      });

      // get encoder host to run batch
      var encoderHost = app.getEncoderHost();
      if (encoderHost) {
        encoderHost.runBatch();
      } else {
        $.writeln("encoderHost not valid");
      }
    } else {
      $.writeln("encoderWrapper not valid");
    }
  } else {
    $.writeln("the project doesn't have any comps / sequences");
  }
} else {
  $.writeln("frontend not valid");
}
```

</details><br><details>

<summary>addFileSequenceToBatch Example (click to expand):</summary>
```javascript
var firstFile = "C:\\testdata\\Images\\AB-1.jpg";
var preset = "C:\\testdata\\HighQuality720HD.epr";
var destination = "C:\\testdata\\outputFolder";
var inContainingFolder = "C:\\testdata\\Images";

// //sources for mac
// var firstFile = "/Users/Shared/testdata/Images/AB-1.jpg"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";
// var destination = "/Users/Shared/testdata/outputFolder";
// var inContainingFolder = "/Users/Shared/testdata/Images";

var frontend = app.getFrontend();
if (frontend) {
  // listen for batch item added event
  frontend.addEventListener("onItemAddedToBatch", function (eventObj) {
    $.writeln("onAddItemToBatch success");
  });

  var batchItemSuccess = frontend.addFileSequenceToBatch(
    inContainingFolder,
    firstFile,
    preset,
    destination
  );

  if (batchItemSuccess) {
    $.writeln("Batch item added successfully");

    // get encoderHost to be able to listen for the item complete event
    var encoderHost = app.getEncoderHost();
    if (encoderHost) {
      encoderHost.addEventListener("onItemEncodeComplete", function (eventObj) {
        $.writeln("Result: " + eventObj.result);
        $.writeln("Source File Path: " + eventObj.sourceFilePath);
        $.writeln("Output File Path: " + eventObj.outputFilePath);
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

</details><br><details>

<summary>addFileToBatch Example (click to expand):</summary>
```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\HighQuality720HD.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var frontend = app.getFrontend();
if (frontend) {
  // listen for batch item added / creation failed event
  frontend.addEventListener("onItemAddedToBatch", function (eventObj) {
    $.writeln("frontend.onItemAddedToBatch: success");
  });

  frontend.addEventListener("onBatchItemCreationFailed", function (eventObj) {
    $.writeln("frontend.onBatchItemCreationFailed: failed");
    $.writeln("srcFilePath: " + eventObj.srcFilePath);
    $.writeln("error: " + eventObj.error);
  });

  var encoderWrapper = frontend.addFileToBatch(
    source,
    "H.264",
    preset,
    destination
  );
  if (encoderWrapper) {
    $.writeln("Batch item added successfully for source file  ", source);

    // listen for encode progress and encode finish event
    encoderWrapper.addEventListener("onEncodeProgress", function (eventObj) {
      $.writeln("Encoding progress for batch item: " + eventObj.result);
    });

    encoderWrapper.addEventListener("onEncodeFinished", function (eventObj) {
      $.writeln("encoderWrapper.onEncodeFinished Success: " + eventObj.result);
    });

    // get encoder host to run batch
    var encoderHost = app.getEncoderHost();
    if (encoderHost) {
      encoderHost.runBatch();
    } else {
      $.writeln("encoderHost not valid");
    }
  } else {
    $.writeln(
      "encoderWrapper not valid - batch item wasn't added successfully"
    );
  }
} else {
  $.writeln("frontend not valid");
}
```

</details><br><details>

<summary>addItemToBatch Example (click to expand):</summary>
```javascript
var source = "C:\\testdata\\testmedia3.mxf";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"

var frontend = app.getFrontend();
if (frontend) {
  // listen for batch item added event
  frontend.addEventListener("onItemAddedToBatch", function (eventObj) {
    $.writeln("frontend.onItemAddedToBatch: success");
  });

  var batchItemSuccess = frontend.addItemToBatch(source);
  if (batchItemSuccess) {
    $.writeln("Batch item added successfully for the source file ", source);

    // get encoderHost to be able to listen for the item complete event
    encoderHost = app.getEncoderHost();
    if (encoderHost) {
      encoderHost.addEventListener("onItemEncodeComplete", function (eventObj) {
        $.writeln("Result: " + eventObj.result);
        $.writeln("Source File Path: " + eventObj.sourceFilePath);
        $.writeln("Output File Path: " + eventObj.outputFilePath);
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

</details><br><details>

<summary>addTeamProjectsItemToBatch Example (click to expand):</summary>
```javascript
// use for the source (projectsURL) a valid Team Projects URL or a Team Projects Snap
// you can create a tp2snap file in PPro for a ProjectItem via the scripting API saveProjectSnapshot
// e.g. projectItem.saveProjectSnapshot("C:\\testdata\\test.tp2snap");
var format = "H.264";
var teamsProjectPath = "C:\\testdata\\test.tp2snap";
var preset = "C:\\testdata\\HighQuality720HD.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var teamsProjectPath = "/Users/Shared/testdata/test.tp2snap"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var frontend = app.getFrontend();
if (frontend) {
  // listen for batch item added / creation failed event
  frontend.addEventListener("onItemAddedToBatch", function (eventObj) {
    $.writeln("frontend.onItemAddedToBatch: success");
  });

  frontend.addEventListener("onBatchItemCreationFailed", function (eventObj) {
    $.writeln("frontend.onBatchItemCreationFailed: failed");
    $.writeln("srcFilePath: " + eventObj.srcFilePath);
    $.writeln("error: " + eventObj.error);
  });

  var encoderWrapper = frontend.addTeamProjectsItemToBatch(
    teamsProjectPath,
    format,
    preset,
    destination
  );

  if (encoderWrapper) {
    $.writeln(
      "Batch item added successfully for Team Projects url: ",
      teamsProjectPath
    );

    // listen for encode progress and encode finish events
    encoderWrapper.addEventListener("onEncodeProgress", function (eventObj) {
      $.writeln("Encoding progress for batch item: " + eventObj.result);
    });

    encoderWrapper.addEventListener("onEncodeFinished", function (eventObj) {
      $.writeln("Encoding result for batch item: " + eventObj.result);
    });

    // get encoder host to run batch
    var encoderHost = app.getEncoderHost();

    if (encoderHost) {
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

</details><br><details>

<summary>addXMLToBatch Example (click to expand):</summary>
```javascript
var source = "C:\\testdata\\FCP-3.xml"; // Final Cut Pro xml file
var preset = "C:\\testdata\\HighQuality720HD.epr";

// //sources for mac
// var source = "/Users/Shared/testdata/FCP-3.xml"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";

var frontend = app.getFrontend();
if (frontend) {
  // listen for batch item added event
  frontend.addEventListener("onItemAddedToBatch", function (eventObj) {
    $.writeln("onAddItemToBatch success");
  });

  var batchItemsuccess = frontend.addXMLToBatch(source, preset);

  if (batchItemsuccess) {
    $.writeln("Added xml file to batch successfully.");

    // get encoder host to listen for onItemEncodeComplete event and run batch
    encoderHost = app.getEncoderHost();
    if (encoderHost) {
      encoderHost.addEventListener("onItemEncodeComplete", function (eventObj) {
        $.writeln("Result: " + eventObj.result);
        $.writeln("Source File Path: " + eventObj.sourceFilePath);
        $.writeln("Output File Path: " + eventObj.outputFilePath);
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

</details><br><details>

<summary>getDLItemsAtRoot Example (click to expand):</summary>
```javascript
var projectPath = "C:\\testdata\\aeCompTest.aep"; // project path

// //sources for mac
// var projectPath = "/Users/Shared/testdata/aeCompTest.aep"

var frontend = app.getFrontend();
if (frontend) {
  var result = frontend.getDLItemsAtRoot(projectPath);

  $.writeln(result.length + " ae comps found.");
  for (var idx = 0; idx < result.length; ++idx) {
    $.writeln("GUID for item " + idx + " is " + result[idx] + ".");

    // These guids will be needed for e.g. the API frontend.addDLToBatch
  }
} else {
  $.writeln("frontend not valid");
}
```

</details><br><details>

<summary>stitchFiles Example (click to expand):</summary>
```javascript
var format = "H.264";
var media_1 = "C:\\testdata\\testmedia.mp4";
var media_2 = "C:\\testdata\\testmedia2.mp4.";
var preset = "C:\\testdata\\HighQuality720HD.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var media_1 = "/Users/Shared/testdata/testmedia.mp4"
// var media_2 = "/Users/Shared/testdata/testmedia2.avi"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var mediaPaths = media_1 + ";" + media_2;

var frontend = app.getFrontend();
if (frontend) {
  // listen for batch item added / creation failed event
  frontend.addEventListener("onItemAddedToBatch", function (eventObj) {
    $.writeln("onAddItemToBatch success");
  });

  frontend.addEventListener("onBatchItemCreationFailed", function (eventObj) {
    $.writeln("onBatchItemCreationFailed");
  });

  var encoderWrapper = frontend.stitchFiles(
    mediaPaths,
    format,
    preset,
    destination
  );

  if (encoderWrapper) {
    $.writeln("Batch item added successfully");

    // listen for encode progress and encode finish events
    encoderWrapper.addEventListener("onEncodeProgress", function (eventObj) {
      $.writeln("Encoding progress for batch item: " + eventObj.result);
    });

    encoderWrapper.addEventListener("onEncodeFinished", function (eventObj) {
      $.writeln("Encoding result for batch item: " + eventObj.result);
    });

    // get encoder host to run batch
    var encoderHost = app.getEncoderHost();

    if (encoderHost) {
      encoderHost.runBatch();
    } else {
      $.writeln("encoderHost not valid");
    }
  } else {
    $.writeln("encoderWrapper not valid");
  }
} else {
  $.writeln("frontend not valid");
}
```

</details><br>

## SourceMediaInfo

**Get the width, height, PAR, duration, etc about the imported source**

<a id="properties-8"></a>

### Properties

- `audioDuration: string` : Returns the audio duration of the
  source
- `description: string` : Returns embedded description of the
  source
- `dropFrameTimeCode: boolean` : Returns true if the timecode is a
  drop frame timecode
- `duration: string` : Returns duration of the source
- `durationInTicks: None` : Returns duration of the source in
  ticks (available since 23.3)
- `fieldType: string` : Returns field type of the source
- `frameRate: string` : Returns frame rate of the source
- `height: string` : Returns height of the source
- `importer: string` : Returns the importer used to decode the
  source
- `numChannels: string` : Returns the number of audio channels of
  the source
- `parX: string` : Returns the X PAR of the source
- `parY: string` : Returns the Y PAR of the source
- `sampleRate: string` : Returns sample rate of the source
- `width: string` : Returns width of the source
- `xmp: string` : Returns xmp xml of the source

<a id="code-samples-10"></a>

### Examples

<details>

<summary>Example (click to expand):</summary>
```javascript
var source = "C:\\testdata\\testmedia3.mxf";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"

var exporter = app.getExporter();
if (exporter) {
  var sourceMediaInfo = exporter.getSourceMediaInfo(source);
  if (sourceMediaInfo) {
    var audioDuration = sourceMediaInfo.audioDuration;
    $.writeln("audio duration of the source: " + audioDuration);

    var description = sourceMediaInfo.description;
    $.writeln("description of the source: " + description);

    var isDropFrame = sourceMediaInfo.dropFrameTimeCode;
    $.writeln("is drop frame: " + dropFrameTimeCode);

    var duration = sourceMediaInfo.duration;
    $.writeln("duration of the source: " + duration);

    var fieldType = sourceMediaInfo.fieldType;
    $.writeln("field type of the source: " + fieldType);

    var frameRate = sourceMediaInfo.frameRate;
    $.writeln("frame rate of the source: " + frameRate);

    var height = sourceMediaInfo.height;
    $.writeln("height of the source: " + height);

    var importer = sourceMediaInfo.importer;
    $.writeln("importer of the source: " + importer);

    var numChannels = sourceMediaInfo.numChannels;
    $.writeln("num channels of the source: " + numChannels);

    var parX = sourceMediaInfo.parX;
    $.writeln("par X of the source: " + parX);

    var parY = sourceMediaInfo.parY;
    $.writeln("par Y of the source: " + parY);

    var sampleRate = sourceMediaInfo.sampleRate;
    $.writeln("sample rate of the source: " + sampleRate);

    var width = sourceMediaInfo.width;
    $.writeln("width of the source: " + width);

    var xmp = sourceMediaInfo.xmp;
    $.writeln("xmp of the source: " + xmp);
  }
}
```

</details><br>



## WatchFolderScriptObject

**Scripting methods to watch folders**

<a id="methods-5"></a>

### Methods

- `createWatchFolder(folderPath: string, outputPath: string, presetPath: string): boolean` : Create
  a watch folder at destination for the preset and add the source
  - `folderPath`: The path to the folder which should be added as
    watch folder
- `removeAllWatchFolders(): boolean` : Remove all watch folders

<a id="code-samples-11"></a>

### Examples

<details>

<summary>createWatchFolder Example (click to expand):</summary>
```javascript
var folder = "C:\\testdata\\watchFolder";
var preset = "C:\\testdata\\HighQuality720HD.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var folder = "/Users/Shared/testdata/watchFolder"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var watchFolder = app.getWatchFolder();
if (watchFolder) {
  var watchFolderSuccess = watchFolder.createWatchFolder(
    folder,
    destination,
    preset
  );

  if (watchFolderSuccess) {
    $.writeln(folder, " added as a watch folder");
    encoderHostWrapper = app.getEncoderHost();
    if (encoderHostWrapper) {
      watchFolder.addEventListener("onEncodeComplete", function (eventObj) {
        $.writeln("Elapsed Time: " + eventObj.elapsedTime);
        $.writeln("watchFolder.onEncodeComplete");
      });

      watchFolder.addEventListener("onEncodeError", function (eventObj) {
        $.writeln("watchFolder.onEncodeError");
      });

      encoderHostWrapper.runBatch();
    } else {
      $.writeln("EncoderHostWrapper object is not valid");
    }
  } else {
    $.writeln("Watch folder was not created");
  }
}
```

</details><br><details>

<summary>removeAllWatchFolders Example (click to expand):</summary>
```javascript
var folder = "C:\\testdata\\watchFolder";
var preset = "C:\\testdata\\HighQuality720HD.epr";
var destination = "C:\\testdata\\outputWatchfolder1";
var folder2 = "C:\\testdata\\watchFolder2";
var destination2 = "C:\\testdata\\outputWatchfolder2";

// //sources for mac
// var folder = "/Users/Shared/testdata/watchFolder"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";
// var destination = "/Users/Shared/testdata/outputWatchfolder1";
// var folder2 = "/Users/Shared/testdata/watchFolder2"
// var destination2 = "/Users/Shared/testdata/outputWatchfolder2";

var watchFolderObj = app.getWatchFolder();
if (watchFolderObj) {
  watchFolder.createWatchFolder(folder, destination, preset);
  watchFolder.createWatchFolder(folder2, destination2, preset);
  watchFolderObj.removeAllWatchFolders();
} else {
  $.writeln("Watch folder object is not valid");
}
```

</details><br>
