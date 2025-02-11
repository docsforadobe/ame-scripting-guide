# EncoderWrapper Object

`app.getFrontend().addDLToBatch(...)`
<br/>
`app.getFrontend().addFileToBatch(...)`
<br/>
`app.getFrontend().addTeamProjectsItemToBatch(...)`
<br/>
`app.getFrontend().exportItem(...)`
<br/>
`app.getFrontend().stitchFiles(...)`

Queue item object to set encode properties

#### Example

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

---

## Attributes

### EncoderWrapper.outputFiles

`app.getFrontend().exportItem(...).outputFiles`

#### Description

Gets the list of files the encode generated.

#### Returns

Array of strings

---

### EncoderWrapper.outputHeight

`app.getFrontend().exportItem(...).outputHeight`

#### Description

Gets the height of the encoded output file.

#### Returns

Float

---

### EncoderWrapper.outputWidth

`app.getFrontend().exportItem(...).outputWidth`

#### Description

Gets the width of the encoded output file.

#### Returns

Float

---

## Methods

### EncoderWrapper.SetIncludeSourceXMP()

`app.getFrontend().exportItem(...).SetIncludeSourceXMP(includeSourceXMP)`

#### Description

Toggle the inclusion of source XMP.

#### Parameters

|     Parameter      |  Type   |          Description          |
| ------------------ | ------- | ----------------------------- |
| `includeSourceXMP` | Boolean | Whether to include source XMP |

#### Returns

Boolean

---

### EncoderWrapper.getEncodeProgress()

`app.getFrontend().exportItem(...).getEncodeProgress()`

#### Description

Returns the encode progress as percentage

#### Returns

Integer

---

### EncoderWrapper.getEncodeTime()

`app.getFrontend().exportItem(...).getEncodeTime()`

#### Description

Return the encode time in milliseconds

#### Returns

Float

---

### EncoderWrapper.getLogOutput()

`app.getFrontend().exportItem(...).getLogOutput()`

!!! note
    This functionality was added in Media Encoder 23.2

#### Description

Returns the log output including possible warnings and errors.

#### Returns

String

#### Example

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

---

### EncoderWrapper.getMissingAssets()

`app.getFrontend().exportItem(...).getMissingAssets(includeSource, includeOutput)`

#### Description

Returns a list of missing assets

#### Parameters

|   Parameter   |  Type   |             Description             |
| ------------- | ------- | ----------------------------------- |
| includeSource | Boolean | Whether to include the source group |
| includeOutput | Boolean | Whether to include the output       |

#### Returns

Array of strings

---

### EncoderWrapper.getPresetList()

`app.getFrontend().exportItem(...).getPresetList()`

#### Description

Returns the presets available for the assigned format

#### Returns

Array of strings

#### Example

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

---

### EncoderWrapper.loadFormat()

`app.getFrontend().exportItem(...).loadFormat(format)`

#### Description

Changes the format for the batch item

#### Parameters

| Parameter |  Type  |               Description               |
| --------- | ------ | --------------------------------------- |
| `format`  | String | The format to change to, e.g. `"H.264"` |

#### Returns

Boolean

#### Example

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

---

### EncoderWrapper.loadPreset()

`app.getFrontend().exportItem(...).loadPreset(presetPath)`

#### Description

Loads and assigns the preset to the batch item

#### Parameters

|  Parameter   |  Type  |          Description          |
| ------------ | ------ | ----------------------------- |
| `presetPath` | String | The path to load presets from |

#### Returns

Boolean

#### Example

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

---

### EncoderWrapper.setCropOffsets()

`app.getFrontend().exportItem(...).setCropOffsets(left, top, right, bottom)`

#### Description

Sets the crop offsets

#### Parameters

| Parameter |     Type     |      Description       |
| --------- | ------------ | ---------------------- |
| `left`    | Unsigned int | The left crop offset   |
| `top`     | Unsigned int | The top crop offset    |
| `right`   | Unsigned int | The right crop offset  |
| `bottom`  | Unsigned int | The bottom crop offset |

#### Returns

Boolean

---

### EncoderWrapper.setCropState()

`app.getFrontend().exportItem(...).setCropState(cropState)`

#### Description

Sets the crop state

#### Parameters

|  Parameter  |  Type   |       Description        |
| ----------- | ------- | ------------------------ |
| `cropState` | Boolean | The crop state to set to |

#### Returns

Boolean

---

### EncoderWrapper.setCropType()

`app.getFrontend().exportItem(...).setCropType(cropType)`

#### Description

Sets the crop type

#### Parameters

+------------+--------------+---------------------------------+
| Parameter  |     Type     |           Description           |
+============+==============+=================================+
| `cropType` | Unsigned int | One of:                         |
|            |              |                                 |
|            |              | - 0 - `ScaleToFit`              |
|            |              | - 1 - `ScaleToFitBeforeCrop`    |
|            |              | - 2 - `SetAsOutputSize`         |
|            |              | - 3 - `ScaleToFill`             |
|            |              | - 4 - `ScaleToFillBeforeCrop`   |
|            |              | - 5 - `StretchToFill`           |
|            |              | - 6 - `StretchToFillBeforeCrop` |
+------------+--------------+---------------------------------+

#### Returns

Boolean

---

### EncoderWrapper.setCuePointData()

`app.getFrontend().exportItem(...).setCuePointData(inCuePointsFilePath)`

#### Description

Sets the cue point data

#### Parameters

|       Parameter       |  Type  |          Description          |
| --------------------- | ------ | ----------------------------- |
| `inCuePointsFilePath` | string | File path for the data to set |

#### Returns

Boolean

---

### EncoderWrapper.setFrameRate()

`app.getFrontend().exportItem(...).setFrameRate(framerate)`

#### Description

Sets the frame rate for the batch item

#### Parameters

|  Parameter  |  Type  |               Description               |
| ----------- | ------ | --------------------------------------- |
| `framerate` | String | The frame rate, as a string e.g. `"24"` |

#### Returns

Boolean

---

### EncoderWrapper.setIncludeSourceCuePoints()

`app.getFrontend().exportItem(...).setIncludeSourceCuePoints(includeSourceCuePoints)`

#### Description

Toggle the inclusion of cue points

#### Parameters

|        Parameter         |  Type   |             Description              |
| ------------------------ | ------- | ------------------------------------ |
| `includeSourceCuePoints` | Boolean | Whether to include source cue points |

#### Returns

Boolean

---

### EncoderWrapper.setOutputFrameSize()

`app.getFrontend().exportItem(...).setOutputFrameSize(width, height)`

#### Description

Sets the output frame size

#### Parameters

| Parameter |     Type     | Description  |
| --------- | ------------ | ------------ |
| `width`   | Unsigned int | Frame width  |
| `height`  | Unsigned int | Frame height |

#### Returns

Boolean

---

### EncoderWrapper.setRotation()

`app.getFrontend().exportItem(...).setRotation(rotationValue)`

#### Description

Sets the rotation (in a 360 degree system)

#### Parameters

|    Parameter    | Type  |                 Description                 |
| --------------- | ----- | ------------------------------------------- |
| `rotationValue` | Float | Rotation value, in the range `[0.0..360.0]` |

#### Returns

Boolean

---

### EncoderWrapper.setScaleType()

`app.getFrontend().exportItem(...).setScaleType(scaleType)`

#### Description

Sets the scale type

#### Parameters

+-------------+--------------+---------------------------------+
|  Parameter  |     Type     |           Description           |
+=============+==============+=================================+
| `scaleType` | Unsigned int | One of:                         |
|             |              |                                 |
|             |              | - 0 - `ScaleToFit`              |
|             |              | - 1 - `ScaleToFitBeforeCrop`    |
|             |              | - 2 - `SetAsOutputSize`         |
|             |              | - 3 - `ScaleToFill`             |
|             |              | - 4 - `ScaleToFillBeforeCrop`   |
|             |              | - 5 - `StretchToFill`           |
|             |              | - 6 - `StretchToFillBeforeCrop` |
+-------------+--------------+---------------------------------+

#### Returns

Boolean

---

### EncoderWrapper.setTimeInterpolationType()

`app.getFrontend().exportItem(...).setTimeInterpolationType(interpolationType)`

#### Description

Set the time interpolation type

#### Parameters

+---------------------+--------------+-----------------------+
|      Parameter      |     Type     |      Description      |
+=====================+==============+=======================+
| `interpolationType` | Unsigned int | One of:               |
|                     |              |                       |
|                     |              | - 0 - `FrameSampling` |
|                     |              | - 1 - `FrameBlending` |
|                     |              | - 2 - `OpticalFlow`   |
+---------------------+--------------+-----------------------+


#### Returns

Boolean

---

### EncoderWrapper.setUseFrameBlending()

`app.getFrontend().exportItem(...).setUseFrameBlending(useFrameBlending)`

#### Description

Toggle the use of frame blending.

#### Parameters

|     Parameter      |  Type   |          Description          |
| ------------------ | ------- | ----------------------------- |
| `useFrameBlending` | Boolean | Whether to use frame blending |

#### Returns

Boolean

---

### EncoderWrapper.setUseMaximumRenderQuality()

`app.getFrontend().exportItem(...).setUseMaximumRenderQuality(useMaximumRenderQuality)`

#### Description

Toggle the use of maximum render quality.

#### Parameters

|         Parameter         |  Type   |              Description              |
| ------------------------- | ------- | ------------------------------------- |
| `useMaximumRenderQuality` | Boolean | Whether to use maximum render quality |

#### Returns

Boolean

---

### EncoderWrapper.setUsePreviewFiles()

`app.getFrontend().exportItem(...).setUsePreviewFiles(usePreviewFiles)`

#### Description

Toggle the use of previews files.

#### Parameters

|     Parameter     |  Type   |         Description          |
| ----------------- | ------- | ---------------------------- |
| `usePreviewFiles` | Boolean | Whether to use preview files |

#### Returns

Boolean

---

### EncoderWrapper.setWorkArea()

`app.getFrontend().exportItem(...).setWorkArea(workAreaType, startTime, endTime)`

#### Description

Sets the work area type, start and end time for the batch item.

#### Parameters

+----------------+--------------+--------------------+
|   Parameter    |     Type     |    Description     |
+================+==============+====================+
| `workAreaType` | Unsigned int | One of:            |
|                |              |                    |
|                |              | - 0 - `Entire`     |
|                |              | - 1 - `InToOut`    |
|                |              | - 2 - `WorkArea`   |
|                |              | - 3 - `Custom`     |
|                |              | - 4 - `UseDefault` |
+----------------+--------------+--------------------+
| `startTime`    | Float        | Start time         |
+----------------+--------------+--------------------+
| `endTime`      | Float        | End time           |
+----------------+--------------+--------------------+

#### Returns

Boolean

---

### EncoderWrapper.setWorkAreaInTicks()

`app.getFrontend().exportItem(...).setWorkAreaInTicks(workAreaType, startTime, endTime)`

!!! note
    This functionality was added in Media Encoder 23.3

#### Description

Sets the work area type, start and end time in ticks for the batch item

#### Parameters

+----------------+--------------+----------------------+
|   Parameter    |     Type     |     Description      |
+================+==============+======================+
| `workAreaType` | Unsigned int | One of:              |
|                |              |                      |
|                |              | - 0 - `Entire`       |
|                |              | - 1 - `InToOut`      |
|                |              | - 2 - `WorkArea`     |
|                |              | - 3 - `Custom`       |
|                |              | - 4 - `UseDefault`   |
|                |              |                      |
+----------------+--------------+----------------------+
| `startTime`    | Float        | Start time, in ticks |
+----------------+--------------+----------------------+
| `endTime`      | Float        | End time, in ticks   |
+----------------+--------------+----------------------+

#### Returns

Boolean

#### Example

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

---

### EncoderWrapper.setXMPData()

`app.getFrontend().exportItem(...).setXMPData(templateXMPFilePath)`

#### Description

Sets XMP data to given template

#### Parameters

|       Parameter       |  Type  |          Description          |
| --------------------- | ------ | ----------------------------- |
| `templateXMPFilePath` | String | File path to the XMP template |

#### Returns

Boolean

---

## Events

An event to inform of encode progress and completion.

### Example

```javascript
var source = "C:\\testdata\\testmedia3.mxf";
var preset = "C:\\testdata\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia3.mxf"
// var preset = "/Users/Shared/testdata/HighQuality720HD.epr";
// var destination = "/Users/Shared/testdata/outputFolder";

var exporter = app.getExporter();
if (exporter) {
  var encoderWrapper = exporter.exportItem(source, destination, preset);
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

    // listen to the audio pre-encoding progress event (available since 24.0.)
    encoderWrapper.addEventListener(
      "onAudioPreEncodeProgress",
      function (eventObj) {
        $.writeln("Audio pre-encoding info: " + eventObj.audioInfo);
        $.writeln("Audio pre-encoding progress: " + eventObj.audioProgress);
      },
      false
    );
  }
}
```

---

### onAudioPreEncodeProgress

`app.getFrontend().exportItem(...).addEventListener("onAudioPreEncodeProgress")`

!!! note
    This functionality was added in Media Encoder 24.0

#### Description

Notify when the audio pre-encode progress changes.

#### Properties

|    Property     |  Type  |               Description               |
| --------------- | ------ | --------------------------------------- |
| `audioInfo`     | String | Returns the audio pre-encoding info     |
| `audioProgress` | String | Returns the audio pre-encoding progress |

---

### onEncodeFinished

`app.getFrontend().exportItem(...).addEventListener("onEncodeFinished")`

#### Description

Notify when the batch item has been encoded.

#### Properties

+----------+--------+--------------------------------------+
| Property |  Type  |             Description              |
+==========+========+======================================+
| `result` | String | Returns the encoding result, one of: |
|          |        |                                      |
|          |        | - `"Done!"`                          |
|          |        | - `"Failed!"`                        |
|          |        | - `"Stopped!"`                       |
+----------+--------+--------------------------------------+

---

### onEncodeProgress

`app.getFrontend().exportItem(...).addEventListener("onEncodeProgress")`

#### Description

Notify when the batch item encode progress changes.

#### Properties

| Property |  Type  |                     Description                      |
| -------- | ------ | ---------------------------------------------------- |
| `result` | String | Returns the encoding result, in the range `[0..100]` |
