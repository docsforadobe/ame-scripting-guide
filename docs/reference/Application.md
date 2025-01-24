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

`cancelTask(taskID: int)`

#### Description

Cancel the task that matches the task ID

#### Returns

Boolean

---

### app.getEncoderHost()

`getEncoderHost()`

#### Description

Get the encoder host object. See EncoderHostScriptObject

#### Returns

Scripting object

---

### app.getExporter()

`getExporter()`

#### Description

Get the exporter object. See `ExporterScriptObject`

#### Returns

Scripting object

---

### app.getFrontend()

`getFrontend()`

#### Description

Get the front end object. See `FrontendScriptObject`

#### Returns

Scripting object

---

### app.getWatchFolder()

`getWatchFolder()`

#### Description

Get the watch folder object. See `WatchFolderScriptObject`

#### Returns

Scripting object

---

### app.isBlackVideo()

`isBlackVideo(sourcePath: string)`

#### Description

True if all frames are black

#### Returns

Boolean

---

### app.isSilentAudio()

`isSilentAudio(sourcePath: string)`

#### Description

True if audio is silent

#### Returns

Boolean

---

### app.quit()

`quit()`

#### Description

Quit the AME app

#### Returns

Boolean

---

### app.renderFrameSequence()

`renderFrameSequence(sourcePath: string, outputPath: string, renderAll: boolean, startFrame: int)`

#### Description

Render still frames for given source

#### Returns

Boolean

---

### app.scheduleTask()

`scheduleTask(scriptToExecute: string, delayInMilliseconds: int, repeat: boolean)`

#### Description

Schedule a script to run after delay, returns task ID

`scriptToExecute`: Put your script as text, e.g. `app.getEncoderHost().runBatch()`.

#### Returns

Int

---

### app.wait()

`wait(milliseconds: unsigned int)`

#### Description

Non UI blocking wait in milliseconds

#### Returns

Boolean

---

### app.write()

`write(text: string)`

#### Description

Write text to std out

#### Returns

Boolean

---

## Examples

### getExporter Example

```javascript
var exporter = app.getExporter();
// check ExporterScriptObject to see which methods/properties you can apply
```

### isBlackVideo Example

```javascript
var source = "C:\\testdata\\testmedia.mp4";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia.mp4"

var blackVideo = app.isBlackVideo(source);
if (blackVideo) {
  $.writeln("The input file has only black frames");
}
```

### isSilentAudio Example

```javascript
var source = "C:\\testdata\\testmedia.mp4";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia.mp4"

var silent = app.isSilentAudio(source);
if (silent) {
  $.writeln("The input file has no audio");
}
```

### renderFrameSequence Example

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

### scheduleTask Example

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
