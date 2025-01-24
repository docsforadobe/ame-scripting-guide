# SourceMediaInfo Object

`app.getExporter().getSourceMediaInfo(source)`

Holds various information about a given source

---

## Properties

### SourceMediaInfo.audioDuration

`app.getExporter().getSourceMediaInfo(source).audioDuration`

#### Description

Returns the audio duration of the source

#### Type

String

---

### SourceMediaInfo.description

`app.getExporter().getSourceMediaInfo(source).description`

#### Description

Returns embedded description of the source

#### Type

String

---

### SourceMediaInfo.dropFrameTimeCode

`app.getExporter().getSourceMediaInfo(source).dropFrameTimeCode`

#### Description

Returns true if the timecode is a drop frame timecode

#### Type

Boolean

---

### SourceMediaInfo.duration

`app.getExporter().getSourceMediaInfo(source).duration`

#### Description

Returns duration of the source

#### Type

String

---

### SourceMediaInfo.durationInTicks

`app.getExporter().getSourceMediaInfo(source).durationInTicks`

!!! note
    This functionality was added in Media Encoder 23.3

#### Description

Returns duration of the source in ticks

#### Type

None

---

### SourceMediaInfo.fieldType

`app.getExporter().getSourceMediaInfo(source).fieldType`

#### Description

Returns field type of the source

#### Type

String

---

### SourceMediaInfo.frameRate

`app.getExporter().getSourceMediaInfo(source).frameRate`

#### Description

Returns frame rate of the source

#### Type

String

---

### SourceMediaInfo.height

`app.getExporter().getSourceMediaInfo(source).height`

#### Description

Returns height of the source

#### Type

String

---

### SourceMediaInfo.importer

`app.getExporter().getSourceMediaInfo(source).importer`

#### Description

Returns the importer used to decode the source

#### Type

String

---

### SourceMediaInfo.numChannels

`app.getExporter().getSourceMediaInfo(source).numChannels`

#### Description

Returns the number of audio channels of the source

#### Type

String

---

### SourceMediaInfo.parX

`app.getExporter().getSourceMediaInfo(source).parX`

#### Description

Returns the X PAR of the source

#### Type

String

---

### SourceMediaInfo.parY

`app.getExporter().getSourceMediaInfo(source).parY`

#### Description

Returns the Y PAR of the source

#### Type

String

---

### SourceMediaInfo.sampleRate

`app.getExporter().getSourceMediaInfo(source).sampleRate`

#### Description

Returns sample rate of the source

#### Type

String

---

### SourceMediaInfo.width

`app.getExporter().getSourceMediaInfo(source).width`

#### Description

Returns width of the source

#### Type

String

---

### SourceMediaInfo.xmp

`app.getExporter().getSourceMediaInfo(source).xmp`

#### Description

Returns xmp xml of the source

#### Type

String

---

## Example

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
