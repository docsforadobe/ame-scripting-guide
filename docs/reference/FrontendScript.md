# FrontendScriptObject

Scripting methods for the frontend.

---

## Methods

### FrontendScript.addCompToBatch()

`app.getFrontend().addCompToBatch(compPath, [presetPath], [outputPath])`

#### Description

Adds the first comp of an After Effects project resp. the first sequence of a Premiere Pro project to the batch.

#### Parameters

|  Parameter   |  Type  |                                                Description                                                 |
| ------------ | ------ | ---------------------------------------------------------------------------------------------------------- |
| `compPath`   | String | Path to e.g. an After Effects project or Premiere Pro project. The first comp resp. sequence will be used. |
| `presetPath` | String | Optional. If `presetPath` is empty, then the default preset will be applied.                               |
| `outputPath` | String | Optional. If `outputPath` is empty, then the output file name will be generated based on the comp path.    |

#### Returns

Boolean

#### Example

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

---

### FrontendScript.addDLToBatch()

`app.getFrontend().addDLToBatch(projectPath, format, presetPath, guid, [outputPath])`

#### Description

Adds e.g. an After Effects comp or Premiere Pro sequence to the batch and returns an [EncoderWrapper](./todo.md).

#### Parameters

|   Parameter   |  Type  |                                                               Description                                                                |
| ------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `projectPath` | String | E.g. Premiere Pro or After Effects project path.                                                                                         |
| `format`      | String | E.g. `"H.264"`                                                                                                                           |
| `presetPath`  | String | Either a preset or a format input must be present. If no preset is used then the default preset of the specified format will be applied. |
| `guid`        | String | The unique id of e.g. a Premiere Pro sequence or After Effects composition.                                                              |
| `outputPath`  | String | Optional. If `outputPath` is empty, then the output file name will be generated based on the project path.                               |

#### Returns

[EncoderWrapper object](./todo.md)

#### Example

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

---

### FrontendScript.addFileSequenceToBatch()

`app.getFrontend().addFileSequenceToBatch(containingFolder, imagePath, presetPath, [outputPath])`

#### Description

Adds an image sequence to the batch. The images will be sorted in alphabetical order.

#### Parameters

|     Parameter      |  Type  |                                                               Description                                                                |
| ------------------ | ------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `containingFolder` | String | The folder containing image files.                                                                                                       |
| `imagePath`        | String | All images from the containing folder with the same extension will be added to the output file.                                          |
| `presetPath`       | String | Either a preset or a format input must be present. If no preset is used then the default preset of the specified format will be applied. |
| `outputPath`       | String | Optional. If `outputPath` is empty, then the output file name will be generated based on the containingFolder name.                      |

#### Returns

Boolean

#### Example

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

---

### FrontendScript.addFileToBatch()

`app.getFrontend().addFileToBatch(filePath, format, presetPath, [outputPath])`

#### Description

Adds a file to the batch and returns an [EncoderWrapper](./todo.md).

#### Parameters

|  Parameter   |  Type  |                                                               Description                                                                |
| ------------ | ------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `filePath`   | String | File path of a media source.                                                                                                             |
| `format`     | String | E.g. `"H.264"`                                                                                                                           |
| `presetPath` | String | Either a preset or a format input must be present. If no preset is used then the default preset of the specified format will be applied. |
| `outputPath` | String | Optional. If `outputPath` is empty, then the output file name will be generated based on the file path.                                  |

#### Returns

[EncoderWrapper object](./todo.md)

#### Example

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

---

### FrontendScript.addItemToBatch()

`app.getFrontend().addItemToBatch(sourcePath)`

#### Description

Adds a media source to the batch.

#### Parameters

|  Parameter   |  Type  |        Description        |
| ------------ | ------ | ------------------------- |
| `sourcePath` | String | Path of the media source. |

#### Returns

Boolean

#### Example

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

---

### FrontendScript.addTeamProjectsItemToBatch()

`app.getFrontend().addTeamProjectsItemToBatch(projectsURL, format, presetPath, [outputPath])`

#### Description

Adds a team project item to the batch and returns an [EncoderWrapper](./todo.md).

#### Parameters

|   Parameter   |  Type  |                                                                 Description                                                                 |
| ------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `projectsURL` | String | Team Projects URL or Team Projects Snap. You can create a tp2snap file in PPro for a ProjectItem via the scripting API saveProjectSnapshot. |
| `format`      | String | E.g. `"H.264"`                                                                                                                              |
| `presetPath`  | String | Either a preset or a format input must be present. If no preset is used then the default preset of the specified format will be applied.    |
| `outputPath`  | String | Optional. If `outputPath` is empty, then the output file name will be generated based on the comp path.                                     |

#### Returns

[EncoderWrapper object](./todo.md)

#### Example

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

---

### FrontendScript.addXMLToBatch()

`app.getFrontend().addXMLToBatch(xmlPath, presetPath, [outputFolderPath])`

#### Description

Adds Final Cut Pro xml to the batch.

#### Parameters

|     Parameter      |  Type  |                                                    Description                                                    |
| ------------------ | ------ | ----------------------------------------------------------------------------------------------------------------- |
| `xmlPath`          | String | Path to a Final Cut Pro xml file.                                                                                 |
| `outputFolderPath` | String | Optional. If `outputFolderPath` is empty, then the output file name will be generated based on the XML file path. |

#### Returns

Boolean

#### Example

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

---

### FrontendScript.getDLItemsAtRoot()

`app.getFrontend().getDLItemsAtRoot(projectPath)`

#### Description

Returns the list of GUIDs for objects (sequences/comps) at the top/root level.

#### Parameters

|   Parameter   |  Type  |                   Description                    |
| ------------- | ------ | ------------------------------------------------ |
| `projectPath` | String | E.g. Premiere Pro or After Effects project path. |

#### Returns

Array of strings

#### Example

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

---

### FrontendScript.stitchFiles()

`app.getFrontend().stitchFiles(mediaPaths, format, presetPath, [outputPath])`

#### Description

Adds a batch item for the given media and returns an [EncoderWrapper](./todo.md).

#### Parameters

|  Parameter   |  Type  |                                                               Description                                                                |
| ------------ | ------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `mediaPaths` | String | Semicolon delimited list of media paths.                                                                                                 |
| `format`     | String | E.g. `"H.264"`                                                                                                                           |
| `presetPath` | String | Either a preset or a format input must be present. If no preset is used then the default preset of the specified format will be applied. |
| `outputPath` | String | Optional. If `outputPath` is empty, then the output file name will be generated based on the comp path.                                  |

#### Returns

[EncoderWrapper object](./todo.md)

#### Example

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

---

### FrontendScript.stopBatch()

`app.getFrontend().stopBatch()`

#### Description

Stops the batch.

#### Returns

Boolean

---

## Events

### onBatchItemCreationFailed

`app.getFrontend().addEventListener("onBatchItemCreationFailed")`

The event will be sent after batch item creation failed.

Can be used for the following [FrontendScript Object](#frontendscriptobject) APIs:

- [FrontendScript.addFileToBatch()](#frontendscriptaddfiletobatch)
- [FrontendScript.addTeamProjectsItemToBatch()](#frontendscriptaddteamprojectsitemtobatch)
- [FrontendScript.addDLToBatch()](#frontendscriptadddltobatch)

#### Properties

|   Property    |  Type  |     Description      |
| ------------- | ------ | -------------------- |
| `error`       | String | The error string     |
| `srcFilePath` | String | The source file path |

#### Example

```javascript
var source = "C:\\testdata\\testmedia.mp4";

// sources for mac
// var source = "/Users/Shared/testdata/testmedia.mp4"

var frontend = app.getFrontend();
if (frontend) {
  frontend.addEventListener("onBatchItemCreationFailed", function (eventObj) {
    $.writeln("Sourcefile", eventObj.srcFilePath);
    $.writeln("onBatchItemCreationFailed: error", eventObj.error);
  });

  var batchItemSuccess = frontend.addItemToBatch(source);
  if (batchItemSuccess) {
    $.writeln(source, " has been added successfully");
  }
}
```

---

### onItemAddedToBatch

`app.getFrontend().addEventListener("onItemAddedToBatch")`

The event will be sent after a batch item has been created successfully.

Can be used for all [FrontendScript Object](#frontendscriptobject) APIs which creates a batch item.

#### Example

```javascript
var source = "C:\\testdata\\testmedia.mp4";

// //sources for mac
// var source = "/Users/Shared/testdata/testmedia.mp4"

var frontend = app.getFrontend();
if (frontend) {
  frontend.addEventListener("onItemAddedToBatch", function (eventObj) {
    $.writeln("Item added to Batch");
  });

  var batchItemSuccess = frontend.addItemToBatch(source);
  if (batchItemSuccess) {
    $.writeln(source, " has been added successfully");
  }
}
```
