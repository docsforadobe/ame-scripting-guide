# Application

`app`

Top level app object

---

## Attributes

### app.buildNumber

`app.buildNumber`

#### Description

Application build number

#### Type

String

---

## Methods

### app.assertToConsole()

`assertToConsole()`

#### Description

Redirect assert output to stdout.

#### Returns

Boolean

---

### app.bringToFront()

`bringToFront()`

#### Description

Bring application to front

#### Returns

Boolean

---

### app.cancelTask()

`cancelTask(taskID)`

#### Description

Cancel the task that matches the task ID

#### Parameters

| Parameter |  Type   |      Description      |
| --------- | ------- | --------------------- |
| `taskID`  | Integer | The task ID to cancel |

#### Returns

Boolean

---

### app.getEncoderHost()

`getEncoderHost()`

#### Description

Get the encoder host object.

#### Returns

[EncoderHostScript Object](./todo.md)

---

### app.getExporter()

`getExporter()`

#### Description

Get the exporter object.

#### Returns

[ExporterScript Object](./todo.md)

#### Example

```javascript
var exporter = app.getExporter();
// check ExporterScriptObject to see which methods/properties you can apply
```

---

### app.getFrontend()

`getFrontend()`

#### Description

Get the front end object.

#### Returns

[FrontendScript Object](./todo.md)

---

### app.getWatchFolder()

`getWatchFolder()`

#### Description

Get the watch folder object.

#### Returns

[WatchFolderScript Object](./todo)

---

### app.isBlackVideo()

`isBlackVideo(sourcePath)`

#### Description

Checks whether all frames of a video are black.

#### Parameters

|  Parameter   |  Type  |          Description          |
| ------------ | ------ | ----------------------------- |
| `sourcePath` | String | Source path of video to check |

#### Returns

Boolean; `true` if all frames are black

### Example

```javascript
var source = "C:\\testdata\\testmedia.mp4";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia.mp4"

var blackVideo = app.isBlackVideo(source);
if (blackVideo) {
  $.writeln("The input file has only black frames");
}
```

---

### app.isSilentAudio()

`isSilentAudio(sourcePath)`

#### Description

Checks whether a file has no audio

#### Parameters

|  Parameter   |  Type  |         Description          |
| ------------ | ------ | ---------------------------- |
| `sourcePath` | String | Source path of file to check |

#### Returns

Boolean; `true` if audio is silent

#### Example

```javascript
var source = "C:\\testdata\\testmedia.mp4";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia.mp4"

var silent = app.isSilentAudio(source);
if (silent) {
  $.writeln("The input file has no audio");
}
```

---

### app.quit()

`quit()`

#### Description

Quit the AME app

#### Returns

Boolean

---

### app.renderFrameSequence()

`renderFrameSequence(sourcePath, outputPath, renderAll, startFrame)`

#### Description

Render still frames for given source

#### Parameters

|  Parameter   |  Type   |          Description          |
| ------------ | ------- | ----------------------------- |
| `sourcePath` | String  | Source path of file to render |
| `outputPath` | String  | Output path to render to      |
| `renderAll`  | Boolean | Whether to render all frames  |
| `startFrame` | Integer | Start frame to render from    |

#### Returns

Boolean

#### Example

```javascript
var source = "C:\\testdata\\testmedia4.mp4";
var destination = "C:\\testdata\\outputFolder";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia4.mp4"
// var destination = "/Users/Shared/testdata/outputFolder/output.mp4;

var renderall = true;
var startTime = 0;
var success = app.renderFrameSequence(
  source,
  destination,
  renderall,
  startTime
);
if (success) {
  $.writeln("renderFrameSequence() successfully done");
}
```

---

### app.scheduleTask()

`scheduleTask(scriptToExecute, delayInMilliseconds, repeat)`

#### Description

Schedule a script to run after delay

#### Parameters

|       Parameter       |  Type   |                         Description                         |
| --------------------- | ------- | ----------------------------------------------------------- |
| `scriptToExecute`     | String  | Your script as text, e.g. `app.getEncoderHost().runBatch()` |
| `delayInMilliseconds` | Integer | Number of milliseconds to delay before rendering            |
| `repeat`              | Boolean | Whether to repeat the schedule                              |

#### Returns

Integer, the task ID

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
    var taskID = app.scheduleTask(
      "var e = app.getEncoderHost(); e.runBatch()",
      5000,
      false
    );
  } else {
    $.writeln("Encoder wrapper object is not valid.");
  }
} else {
  $.writeln("Frontend object is not valid.");
}
```

---

### app.wait()

`wait(milliseconds)`

#### Description

Non UI blocking

#### Parameters

|   Parameter    |     Type     |            Description             |
| -------------- | ------------ | ---------------------------------- |
| `milliseconds` | Unsigned int | The number of milliseconds to wait |

#### Returns

Boolean

---

### app.write()

`write(text)`

#### Description

Write text to std out

#### Parameters

| Parameter |  Type  |    Description    |
| --------- | ------ | ----------------- |
| `text`    | String | The text to write |

#### Returns

Boolean
