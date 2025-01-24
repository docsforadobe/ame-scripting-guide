# Overview

This is a community-created and -maintained project documenting Extendscript usage for Adobe Media Encoder.

## How do I run scripts in AME?

Similarly to Premiere Pro, you can use Visual Studio Code and the “ExtendScript Debugger” extension to send scripts from VS Code to AME and debug them in the app. Open the script, select the target and run or debug the script.

You can also launch scripts from the command line on Mac and Windows, like this:

```bash
<fullPathToAMEbinary> --console es.processFile <fullPathToScript>
```

Example for executing a test script on Mac:

```bash
"/Applications/Adobe Media Encoder (Beta)/Adobe Media Encoder (Beta).app/
 Contents/MacOS/Adobe Media Encoder (Beta)"
 --console es.processFile ~/Desktop/test.js
```

## What is a good starting point to understand the scripting objects?

Lets start with a very basic script:

```javascript
// make sure to replace "\\" by "/" on Mac with a valid path
var source = "D:\\full\\path\\to\\camera3.mxf";
var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
var destination = "C:\\full\\path\\to\\Output\\test";

var exporter = app.getExporter();

if (exporter) {
   var encoderWrapper  = exporter.exportItem(source, destination, preset);

   exporter.addEventListener("onEncodeComplete", function(eventObj) {
        // We can get the encoding status from the event or from the exporter
        $.writeln("Encode Complete Status: " + eventObj.encodeCompleteStatus);

        var encodeSuccess = exporter.encodeSuccess;
        $.writeln("Encode Complete Status alt: " + encodeSuccess);
    }, false)

    exporter.addEventListener("onError", function(eventObj) {
        // We can get the encoding status from the event or from the exporter
        $.writeln("Error while encoding");

        var encodeSuccess = exporter.encodeSuccess;
        $.writeln("Encode Complete Status: " + encodeSuccess);
    }, false)

}
```

In order to encode a source file in AME, you need to provide the paths of the source file and destination folder, and the preset to be used.

The event listener for `onEncodeComplete` will be called once the encode has successfully finished.

## Where can I ask more questions and get help?

Got more questions than what is covered here? Head over to the Adobe Media Encoder forum here: [https://community.adobe.com/t5/adobe-media-encoder/ct-p/ct-media-encoder](https://community.adobe.com/t5/adobe-media-encoder/ct-p/ct-media-encoder)
