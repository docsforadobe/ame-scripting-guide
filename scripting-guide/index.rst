Adobe Media Encoder (AME) Scripting API Reference Guide
=======================================================

Revision date: 2022-10-10

AMEBatchItemCreationFailedEvent
-------------------------------

**The event will be sent after batch item creation failed. Can be used
for the following FrontendScriptObject API’s: ‘addFileToBatch’,
‘addTeamProjectsItemToBatch’ and ‘addDLToBatch’.**

Properties
~~~~~~~~~~

-  ``error: string`` : Get the error string
-  ``onBatchItemCreationFailed: constant string`` : Notify when the
   batch item creation failed.
-  ``srcFilePath: string`` : Get the source file path.

Code Samples
~~~~~~~~~~~~

.. raw:: html

   <details>

   <summary>onBatchItemCreationFailed Example (click to expand):</summary>

.. code:: javascript

   var source = "C:\\path\\to\\testmedia.mp4";
   var frontend = app.getFrontend();
   var batchItemSuccess = frontend.addItemToBatch(source);

   if (batchItemSuccess) {
     $.writeln(
       "Frontend script engine added the source file ",
       source,
       " successfully"
     );

     frontend.addEventListener("onBatchItemCreationFailed", function (eventObj) {
       $.writeln("frontend.onBatchItemCreationFailed: failed");
     });
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>srcFilePath Example (click to expand):</summary>

.. code:: javascript

   var source = "C:\\path\\to\\testmedia.mp4";
   var frontend = app.getFrontend();
   var batchItemSuccess = frontend.addItemToBatch(source);

   if (batchItemSuccess) {
     $.writeln(
       "Frontend script engine added the source file ",
       source,
       " successfully"
     ); 
     $.writeln("frontend.srcFilePath", frontend.srcFilePath);
   }

.. raw:: html

   </details><br>

AMEExportEvent
--------------

**Provides the following event types: onEncodeComplete, onError,
onMediaInfoCreated, onBatchItemStatusChanged, onItemEncodingStarted,
onEncodingItemProgressUpdated, onPostProcessListInitialized**

.. _properties-1:

Properties
~~~~~~~~~~

-  ``encodeCompleteStatus: bool`` : Returns true after encoding has
   been completed for a batch item. Can be called for onEncodeComplete
   event.
-  ``encodeCompleteTime: float`` : Returns the encoding time in
   milliseconds. Can be called for onEncodeComplete event.
-  ``groupIndex: unsigned int`` : Returns the batch group index. Can
   be called for onBatchItemStatusChanged event.
-  ``itemIndex: unsigned int`` : Returns the batch item index. Can
   be called for onBatchItemStatusChanged event.
-  ``onBatchItemStatusChanged: constant string`` : Notify when batch
   item status has been changed. You can call the API’s groupIndex,
   itemIndex and status for more info.
-  ``onEncodeComplete: constant string`` : Notify when the batch
   item has been encoded. You can call the API’s encodeCompleteStatus
   and encodeCompleteTime for more info.
-  ``onEncodingItemProgressUpdated: constant string`` : Notify the
   encoding progress.
-  ``onError: constant string`` : Notify when there’s an error while
   encoding the batch item.
-  ``onItemEncodingStarted: constant string`` : Notify when the
   encoding of a batch item has started.
-  ``onMediaInfoCreated: constant string`` : Notify when media info
   has been created.
-  ``onPostProcessListInitialized: constant string`` : Notify when
   the post process list is initialized.
-  ``progress: float`` : Returns the batch item encoding progress
   value which is between 0 and 1. Can be called for
   onEncodingItemProgressUpdated event
-  ``status: unsigned int`` : Returns the batch item status. 0 :
   Waiting, 1 : Done, 2 : Failed, 3 : Skipped, 4 : Encoding, 5 : Paused,
   6 : Stopped, 7 : Any, 8 : AutoStart, 9 : Done Warning, 10 : Watch
   Folder Waiting. Can be called for onBatchItemStatusChanged event.

.. _code-samples-1:

Code Samples
~~~~~~~~~~~~

.. raw:: html

   <details>

   <summary>encodeCompleteStatus Example (click to expand):</summary>

.. code:: javascript

   var source = "D:\\Media\\camera3.mxf";
   var preset = "D:\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var destination = "C:\\full\\path\\to\\Output\\test";

   var exporter = app.getExporter();

   if (exporter) {
       exporter.addEventListener("onEncodeComplete", function(eventObj) {
           $.writeln("Encode Complete Status: " + eventObj.encodeCompleteStatus);
       }, false)

       // Alternatively you can access the correct name of that event via the following const property:
       var encodeCompleteEvent = AMEExportEvent.onEncodeComplete;
       exporter.addEventListener(encodeCompleteEvent, function(eventObj) {
           $.writeln("Encode Complete Status (alt): " + eventObj.encodeCompleteStatus);
       }, false)

       var encoderWrapper  = exporter.exportItem(source, destination, preset);
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>encodeCompleteTime Example (click to expand):</summary>

.. code:: javascript

   var source = "D:\\Media\\camera3.mxf";
   var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var destination = "C:\\full\\path\\to\\Output\\test";

   var exporter = app.getExporter();

   if (exporter) {
       exporter.addEventListener("onEncodeComplete", function(eventObj) {
           $.writeln("Encode Complete Time in milli seconds: " + eventObj.encodeCompleteTime);
       }, false)

       // Alternatively you can access the correct name of that event via the following const property:
       var encodeCompleteEvent = AMEExportEvent.onEncodeComplete;
       exporter.addEventListener(encodeCompleteEvent, function(eventObj) {
           $.writeln("Encode Complete Time in milli seconds: (alt): " + eventObj.encodeCompleteTime);
       }, false)

       var encoderWrapper  = exporter.exportItem(source, destination, preset);
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>onBatchItemStatusChanged Example (click to expand):</summary>

.. code:: javascript

   var batchItemStatusChangedEvent = AMEExportEvent.onBatchItemStatusChanged;
   $.writeln("Event name is identical with the const property API name ('onBatchItemStatusChanged'): " + batchItemStatusChangedEvent);

   var source = "D:\\Media\\camera3.mxf";
   var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var destination = "C:\\full\\path\\to\\Output\\test";

   var exporter = app.getExporter();

   if (exporter) {
       exporter.addEventListener(batchItemStatusChangedEvent, function(eventObj) {
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
       }, false)

       // Alternatively you can listen to "onBatchItemStatusChanged" 
       exporter.addEventListener("onBatchItemStatusChanged", function(eventObj) {
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
       }, false)

       var encoderWrapper  = exporter.exportItem(source, destination, preset);
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>onEncodeComplete Example (click to expand):</summary>

.. code:: javascript

   var encodeCompleteEvent = AMEExportEvent.onEncodeComplete;
   $.writeln("Event name is identical with the const property API name ('onEncodeComplete'): " + encodeCompleteEvent);

   var source = "D:\\Media\\camera3.mxf";
   var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var destination = "C:\\full\\path\\to\\Output\\test";

   var exporter = app.getExporter();

   if (exporter) {
       exporter.addEventListener(encodeCompleteEvent, function(eventObj) {
           $.writeln("Encode Complete Status: " + eventObj.encodeCompleteStatus); 
           $.writeln("Encode Complete Time (in milli seconds): " + eventObj.encodeCompleteTime);
       }, false)

       // Alternatively you can listen to "onEncodeComplete" 
       exporter.addEventListener("onEncodeComplete", function(eventObj) {
           $.writeln("Encode Complete Status (alt): " + eventObj.encodeCompleteStatus); 
           $.writeln("Encode Complete Time in milli seconds (alt): " + eventObj.encodeCompleteTime);
       }, false)

       var encoderWrapper  = exporter.exportItem(source, destination, preset);
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>onEncodingItemProgressUpdated Example (click to expand):</summary>

.. code:: javascript

   var encodingItemProgressUpdatedEvent = AMEExportEvent.onEncodingItemProgressUpdated;
   $.writeln("Event name is identical with the const property API name ('onEncodingItemProgressUpdated'): " + encodingItemProgressUpdatedEvent);

   var source = "D:\\Media\\camera3.mxf";
   var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var destination = "C:\\full\\path\\to\\Output\\test";

   var exporter = app.getExporter();

   if (exporter) {
       exporter.addEventListener(encodingItemProgressUpdatedEvent, function(eventObj) {
           $.writeln("Encoding progress for batch item: " + eventObj.progress);
       }, false)

       // Alternatively you can listen to "onEncodingItemProgressUpdated" 
       exporter.addEventListener("onEncodingItemProgressUpdated", function(eventObj) {
           $.writeln("Encoding progress for batch item (alt): " + eventObj.progress);
       }, false)

       var encoderWrapper  = exporter.exportItem(source, destination, preset);
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>onError Example (click to expand):</summary>

.. code:: javascript

   var errorEvent = AMEExportEvent.onError;
   $.writeln("Event name is identical with the const property API name ('onError'): " + errorEvent);

   var source = "D:\\Media\\camera3.mxf";
   var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var destination = "C:\\full\\path\\to\\Output\\test";

   var exporter = app.getExporter();

   if (exporter) {
       exporter.addEventListener(errorEvent, function(eventObj) {
           $.writeln("Error while encoding");
       }, false)

       // Alternatively you can listen to "onError" 
       exporter.addEventListener("onError", function(eventObj) {
           $.writeln("Error while encoding (alt)");
       }, false)

       var encoderWrapper  = exporter.exportItem(source, destination, preset);
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>onItemEncodingStarted Example (click to expand):</summary>

.. code:: javascript

   var itemEncodingStartedEvent = AMEExportEvent.onItemEncodingStarted;
   $.writeln("Event name is identical with the const property API name ('onItemEncodingStarted'): " + itemEncodingStartedEvent);

   var source = "D:\\Media\\camera3.mxf";
   var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var destination = "C:\\full\path\\to\\Output\\test";

   var exporter = app.getExporter();

   if (exporter) {
       exporter.addEventListener(itemEncodingStartedEvent, function(eventObj) {
           $.writeln("Encoding started for batch item.");
       }, false)

       // Alternatively you can listen to "onItemEncodingStarted" 
       exporter.addEventListener("onItemEncodingStarted", function(eventObj) {
           $.writeln("Encoding started for batch item (alt).");
       }, false)

       var encoderWrapper  = exporter.exportItem(source, destination, preset);
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>onMediaInfoCreated Example (click to expand):</summary>

.. code:: javascript

   var mediaInfoCreatedEvent = AMEExportEvent.onMediaInfoCreated;
   $.writeln("Event name is identical with the const property API name ('onMediaInfoCreated'): " + mediaInfoCreatedEvent);

   var source = "D:\\Media\\camera3.mxf";
   var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var destination = "C:\\full\\path\\to\\Output\\test";

   var exporter = app.getExporter();

   if (exporter) {
       exporter.addEventListener(mediaInfoCreatedEvent, function(eventObj) {
           $.writeln("Media info created");
       }, false)

       // Alternatively you can listen to "onMediaInfoCreated" 
       exporter.addEventListener("onMediaInfoCreated", function(eventObj) {
           $.writeln("Media info created (alt)");
       }, false)

       var encoderWrapper  = exporter.exportItem(source, destination, preset);
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>onPostProcessListInitialized Example (click to expand):</summary>

.. code:: javascript

   var postProcessListInitializedEvent = AMEExportEvent.onPostProcessListInitialized;
   $.writeln("Event name is identical with the const property API name ('onPostProcessListInitialized'): " + postProcessListInitializedEvent);

   var source = "D:\\Media\\camera3.mxf";
   var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var destination = "C:\\full\\path\\to\\Output\\test";

   var exporter = app.getExporter();

   if (exporter) {
       exporter.addEventListener(postProcessListInitializedEvent, function(eventObj) {
           $.writeln("Post process list has been initialized.");
       }, false)

       // Alternatively you can listen to "onPostProcessListInitialized" 
       exporter.addEventListener("onPostProcessListInitialized", function(eventObj) {
           $.writeln("Post process list has been initialized (alt).");
       }, false)

       var encoderWrapper  = exporter.exportItem(source, destination, preset);
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>progress Example (click to expand):</summary>

.. code:: javascript

   var source = "D:\\Media\\camera3.mxf";
   var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var destination = "C:\\full\\path\\to\\Output\\test";

   var exporter = app.getExporter();

   if (exporter) {
       exporter.addEventListener("onEncodingItemProgressUpdated", function(eventObj) {
           $.writeln("Encoding progress for batch item: " + eventObj.progress);
       }, false)

       // Alternatively you can access the correct name of that event via the following const property:
       var encodingItemProgressUpdatedEvent = AMEExportEvent.onEncodingItemProgressUpdated;
       exporter.addEventListener(encodingItemProgressUpdatedEvent, function(eventObj) {
           $.writeln("Encoding progress for batch item (alt): " + eventObj.progress);
       }, false)

       var encoderWrapper  = exporter.exportItem(source, destination, preset);
   }


.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>status Example (click to expand):</summary>

.. code:: javascript

   var source = "D:\\Media\\camera3.mxf";
   var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var destination = "C:\\full\\path\\to\\Output\\test";

   var exporter = app.getExporter();

   if (exporter) {
       exporter.addEventListener("onBatchItemStatusChanged", function(eventObj) {
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
       }, false)

       // Alternatively you can access the correct name of that event via the following const property:
       var batchItemStatusChangedEvent = AMEExportEvent.onBatchItemStatusChanged;
       exporter.addEventListener(batchItemStatusChangedEvent, function(eventObj) {
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
       }, false)

       var encoderWrapper  = exporter.exportItem(source, destination, preset);
   }


.. raw:: html

   </details><br>

AMEFrontendEvent
----------------

**The event will be sent after a batch item has been created
successfully.**

.. _properties-2:

Properties
~~~~~~~~~~

-  ``onItemAddedToBatch: constant string`` : Notify when a batch
   item has been created successfully. Can be used for all
   FrontendScriptObject API’s which creates a batch item.

.. _code-samples-2:

Code Samples
~~~~~~~~~~~~

.. raw:: html

   <details>

   <summary>onItemAddedToBatch Example (click to expand):</summary>

.. code:: javascript

   var source = "C:\\full\\path\\to\\testmedia.mp4";
   var frontend = app.getFrontend();
   var batchItemSuccess = frontend.addItemToBatch(source);

   if (batchItemSuccess) {
     $.writeln(
       "Frontend script engine added the source file ",
       source,
       " successfully"
     );

     frontend.addEventListener("onItemAddedToBatch", function (eventObj) {
       $.writeln("frontend.onItemAddedToBatch: success");
     });
   }

.. raw:: html

   </details><br>

Application
-----------

**Top level app object**

.. _properties-3:

Properties
~~~~~~~~~~

-  ``buildNumber: string`` : Get application build number

Methods
~~~~~~~

-  ``assertToConsole(): bool`` : Redirect assert output to stdout.

-  ``bringToFront(): bool`` : Bring application to front

-  ``cancelTask(taskID: int): bool`` : Cancel the task that matches
   the task ID

-  ``getEncoderHost(): scripting object`` : Get the encoder host
   object. See EncoderHostScriptObject

-  ``getExporter(): scripting object`` : Get the exporter object.
   See ExporterScriptObject

-  ``getFrontend(): scripting object`` : Get the front end object.
   See FrontendScriptObject

-  ``getWatchFolder(): scripting object`` : Get the watch folder
   object. See WatchFolderScriptEvent

-  ``isBlackVideo(sourcePath: string): bool`` : True if all frames
   are black

-  ``isSilentAudio(sourcePath: string): bool`` : True if audio is
   silent

-  ``quit(): bool`` : Quit the AME app

-  ``renderFrameSequence(sourcePath: string, outputPath: string, renderAll: bool, startFrame: int): bool`` : Render
   still frames for given source

-  ``scheduleTask(): None`` : 

-  ``wait(milliseconds: unsigned int): bool`` : Non UI blocking wait
   in milliseconds

-  ``write(text: string): bool`` : Write text to std out

.. _code-samples-3:

Code Samples
~~~~~~~~~~~~

.. raw:: html

   <details>

   <summary>getExporter Example (click to expand):</summary>

.. code:: javascript

   var format = "";
   var presetPath = "C:\\full\\path\\to\\HighQuality720HD.epr";
   var testfilePath = "C:\\full\\path\to\\weLove.mp4";

   // for WatchFolder object
   var folder = "C:\\dev\\ExtendScripting\\watchFolder";
   var preset = "C:\\dev\\ExtendScripting\\HighQuality720HD.epr";
   var destination = "C:\\dev\\ExtendScripting\\watchFolder";

   try {
     var frontend = app.getFrontend();
     if (frontend) {
       // Either format or preset can be empty, output is optional
       var encoderWrapper = frontend.addFileToBatch(
         testfilePath,
         format,
         presetPath
       );

       if (encoderWrapper) {
         $.writeln(
           "Frontend script engine added the source file using addFileToBatch-",
           testfilePath,
           " successfully"
         );

         // Starts the encoding
         var encoderHostWrapper = app.getEncoderHost();
         if (encoderHostWrapper) {
           var exporter = app.getExporter();
           if (exporter) {
             exporter.addEventListener(
               "onItemEncodingStarted",
               function (eventObj) {
                 $.writeln("onItemEncodingStarted");
               }
             );

             exporter.addEventListener("onEncodeComplete", function (eventObj) {
               $.writeln(
                 "AMEExportEvent:onEncodeComplete: ",
                 eventObj.encodeCompleteStatus,
                 eventObj.encodeCompleteTime
               );
             });

             encoderHostWrapper.runBatch();
           }
         }
       }
     }
   } catch (error) {
     $.writeln(
       "Something went wrong. Line: ",
       error.line,
       "\t",
       error.description
     );
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>getWatchFolder Example (click to expand):</summary>

.. code:: javascript

   // for WatchFolder object
   var folder = "C:\\full\\path\\to\\watchFolder";
   var preset = "C:\\full\\path\\to\\HighQuality720HD.epr";
   var destination = "C:\\full\\path\\to\\watchFolder";

   try {
     //-----------------------------------------------------------
     // WatchFolderScriptEvent
     //----------------------------------------------------------------------------------------------------------------------
     var watchFolder = app.getWatchFolder();
     if (watchFolder) {
       var watchFolderSuccess = watchFolder.createWatchFolder(
         folder,
         destination,
         preset
       );

       if (watchFolderSuccess) {
         var date = Date();
         $.writeln(
           "WatchfolderScriptObject script engine added the folder to watch: \n",
           folder,
           "\t",
           date
         );

         // this is a global object that sends an oncomplete event once the whole queue is completed
         encoderHostWrapper = app.getEncoderHost();

         if (encoderHostWrapper) {
           encoderHostWrapper.addEventListener(
             "onItemEncodeComplete",
             function (eventObj) {
               $.writeln("encoderHostWrapper.onItemEncodeComplete: success");
             }
           );

           encoderHostWrapper.runBatch();

           watchFolder.addEventListener("onEncodeComplete", function (eventObj) {
             $.writeln("Elapsed Time: " + eventObj.elapsedTime);
             $.writeln("watchFolder.onEncodeComplete: success");
           });

           watchFolder.addEventListener("onEncodeError", function (eventObj) {
             $.writeln("watchFolder.onEncodeError: failed");
           });
         }
       }
     }
   } catch (error) {
     $.writeln(
       "Something went wrong. Line: ",
       error.line,
       "\t",
       error.description
     );
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>isBlackVideo Example (click to expand):</summary>

.. code:: javascript

   var testfilePath = "C:\\full\\path\\to\\Viddeo en negro.mp4";

   try {
     //----------------------------------------------------------
     var blackVideo = app.isBlackVideo(testfilePath);
     if (blackVideo) {
       $.writeln("The input file has only black frames");
     }
   } catch (error) {
     $.writeln(
       "Something went wrong. Line: ",
       error.line,
       "\t",
       error.description
     );
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>isSilentAudio Example (click to expand):</summary>

.. code:: javascript

   var testfilePath = "C:\\full\\path\\to\\NegroImagines Copiar.prproj";

   try {
     //----------------------------------------------------------
     var silent = app.isSilentAudio(testfilePath);
     if (silent) {
       $.writeln("The input file has no audio");
     }
   } catch (error) {
     $.writeln(
       "Something went wrong. Line: ",
       error.line,
       "\t",
       error.description
     );
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>renderFrameSequence Example (click to expand):</summary>

.. code:: javascript

   var testfilePath = "C:\\full\\path\to\\weLove.mp4";
   var outputPath = "C:\\full\\path\\to\\output.mp4";


   try {
     var renderall = true;
     var startTime = 0;
     app.renderFrameSequence(testfilePath, outputPath, renderall, startTime);
   } catch (error) {
     $.writeln(
       "Something went wrong. Line: ",
       error.line,
       "\t",
       error.description
     );
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>scheduleTask Example (click to expand):</summary>

.. code:: javascript

   var format = "";
   var presetPath = "C:\\dev\\ExtendScripting\\HighQuality720HD.epr";
   var testfilePath = "C:\\full\\path\\to\\weLove.mp4";

   try {
     $.writeln("Application build number: ", app.buildNumber);
     var frontend = app.getFrontend();
     if (frontend) {
       // Either format or preset can be empty, output is optional
       var encoderWrapper = frontend.addFileToBatch(
         testfilePath,
         format,
         presetPath
       );

       if (encoderWrapper) {
         var taskID = app.scheduleTask(
           "var e = app.getEncoderHost(); e.runBatch()",
           5000,
           false
         );
       } else {
         throw "Frontend object is not valid.";
       }
     }
   } catch (error) {
     $.writeln(
       "Something went wrong. Line: ",
       error.line,
       "\t",
       error.description
     );
   }

.. raw:: html

   </details><br>

EncoderHostScriptObject
-----------------------

**Provides several utility methods including batch commands to run,
pause or stop the batch.**

.. _methods-1:

Methods
~~~~~~~

-  ``createEncoderForFormat(inFormatName: string): scripting object`` : Returns
   an ‘EncoderWrapper’ script object for the requested format.

-  ``getCurrentBatchPreview(inOutputPath: string): bool`` : Writes
   out the current batch preview image (tiff format) to the given path.

   -  ``inOutputPath``: Path to store a ‘tiff’ file.

-  ``getFormatList(): array of strings`` : Returns a list of all
   available formats.

-  ``getSourceInfo(sourcePath: string): scripting object`` : Returns
   a ‘SourceMediaInfo’ script object which can give detailed info about
   the provided source.

   -  ``sourcePath``: Media path

-  ``getSupportedImportFileTypes(): array of strings`` : Returns
   list of all available formats.

-  ``isBatchRunning(): bool`` : Returns true if a batch job is
   running.

-  ``pauseBatch(): bool`` : Pauses the batch (always returns true).

-  ``runBatch(): bool`` : Runs the batch (always returns true).

-  ``stopBatch(): bool`` : Stops the batch (always returns true).

EncoderHostWrapperEvent
-----------------------

**Provides the following event types: onItemEncodingStarted,
onEncodingItemProgressUpdate, onItemEncodeComplete. For multiple batch
items in the queue we recommend to use this event to ensure that the
event types will be received for all batch items.**

.. _properties-4:

Properties
~~~~~~~~~~

-  ``onEncodingItemProgressUpdate: constant string`` : Notify of the
   batch item encoding progress (available since 23.1.).
-  ``onItemEncodeCompleted: constant string`` : Notify when the
   batch item has been encoded.
-  ``onItemEncodingStarted: constant string`` : Notify when the
   batch item encoding started (available since 23.1.).
-  ``outputFilePath: string`` : Returns the path of the output file.
   Can be called for onItemEncodingStarted and onItemEncodeComplete
   events.
-  ``progress: float`` : Returns the encoding progress between 0 and
   1. Can be called for onEncodingItemProgressUpdate event.
-  ``result: string`` : Returns the encoding result ‘True’ or
   ‘False’. Can be called for onItemEncodeComplete event.
-  ``sourceFilePath: string`` : Returns the path of the source file.
   Can be called for onItemEncodingStarted and onItemEncodeComplete
   events.

.. _code-samples-4:

Code Samples
~~~~~~~~~~~~

.. raw:: html

   <details>

   <summary>Example (click to expand):</summary>

.. code:: javascript

   // Please use this event when you have multiple batch items in the queue (added manually or via a script as below)
   // to ensure you receive all event types
   var source_1 = "D:\\full\\path\\to\\camera1.mxf";
   var source_2 = "D:\\full\\path\\to\\camera2.mxf";
   var source_3 = "D:\\full\\path\\to\\camera3.mxf";

   var frontend = app.getFrontend();
   if (frontend) {

     // listen for batch item added event
     frontend.addEventListener("onItemAddedToBatch", function (eventObj) {
       $.writeln("frontend.onItemAddedToBatch: success");
     });

     var batchItemSuccess_1 = frontend.addItemToBatch(source_1);
     var batchItemSuccess_2 = frontend.addItemToBatch(source_2);
     var batchItemSuccess_3 = frontend.addItemToBatch(source_3);
     if (batchItemSuccess_1 && batchItemSuccess_2 && batchItemSuccess_3) {
       $.writeln(
         "Batch item added successfully for the source files ",
         source_1 + " , " + source_2 + " , " + source_3
       );

       encoderHost = app.getEncoderHost();
       if (encoderHost) {
         // listen to the item encoding started event (available since 23.1.)
         encoderHost.addEventListener(
           "onItemEncodingStarted",
           function (eventObj) {
             $.writeln("onItemEncodingStarted: Source File Path: " + eventObj.sourceFilePath);
             $.writeln("onItemEncodingStarted: Output File Path: " + eventObj.outputFilePath);
           }
         );

         /* for earlier versions (23.0. or older) there's an additional step necessary to listen to the onItemEncodingStarted event
           var exporter = app.getExporter();
           if (exporter) {
               exporter.addEventListener(
                   "onItemEncodingStarted",
                   function (eventObj) {
                   $.writeln("onItemEncodingStarted");
                   }
               );
           }
         */

         // listen to the item encoding progress event (available since 23.1.)
         encoderHost.addEventListener(
           "onEncodingItemProgressUpdate",
           function (eventObj) {
             $.writeln("onEncodingItemProgessUpdate: Encoding Progress: " + eventObj.progress);
           }
         );

          /* for earlier versions (23.0. or older) there's an additional step necessary to listen to the onEncodingItemProgressUpdated event
           var exporter = app.getExporter();
           if (exporter) {
               exporter.addEventListener(
                   "onEncodingItemProgressUpdated",
                   function (eventObj) {
                   $.writeln("onEncodingItemProgessUpdated: Encoding Progress: " + eventObj.progress);
                   }
               );
           }
         */

         // listen to the item encoding complete event
         encoderHost.addEventListener(
           "onItemEncodeComplete",
           function (eventObj) {
             $.writeln("onItemEncodeComplete: Result: " + eventObj.result);
             $.writeln("onItemEncodeComplete: Source File Path: " + eventObj.sourceFilePath);
             $.writeln("onItemEncodeComplete: Output File Path: " + eventObj.outputFilePath);
           }
         );

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

.. raw:: html

   </details><br>

EncoderWrapper
--------------

**Queue item object to set encode properties**

.. _properties-5:

Properties
~~~~~~~~~~

-  ``outputFiles: array of strings`` : Gets the list of files the
   encode generated
-  ``outputHeight: float`` : Gets the height of the encoded output
   file
-  ``outputWidth: float`` : Gets the width of the encoded output
   file

.. _methods-2:

Methods
~~~~~~~

-  ``SetIncludeSourceXMP(includeSourceXMP: bool): bool`` : Toggle
   the inclusion of source XMP [boolean] input value required

-  ``getEncodeProgress(): int`` : Returns the encode progress as
   percentage

-  ``getEncodeTime(): float`` : Return the encode time in
   milliseconds

-  ``getMissingAssets(includeSource: bool, includeOutput: bool): array of strings`` : Returns
   a list of missing assets

   -  ``includeSource``: Get missing asset list from the source group if
      requested

-  ``getPresetList(): array of strings`` : Returns the presets
   available for the assigned format

-  ``loadFormat(format: string): bool`` : Changes the format for the
   the batch item

   -  ``format``: E.g. ‘H.264’ Loads all presets available for the
      assigned format

-  ``loadPreset(presetPath: string): bool`` : Loads and assigns the
   preset to the batch item

-  ``setCropOffsets(left: unsigned int, top: unsigned int, right: unsigned int, bottom: unsigned int): bool`` : Sets
   the crop offsets

-  ``setCropState(cropState: bool): bool`` : Sets the crop state
   [boolean] input value required

-  ``setCropType(cropType: unsigned int): bool`` : Sets the scale
   type

   -  ``cropType``: 0 ScaleToFit, 1 ScaleToFitBeforeCrop, 2
      SetAsOutputSize, 3 ScaleToFill, 4 ScaleToFillBeforeCrop, 5
      StretchToFill, 6 StretchToFillBeforeCrop

-  ``setCuePointData(inCuePointsFilePath: string): bool`` : Sets the
   cue point data

-  ``setFrameRate(framerate: string): bool`` : Sets the frame rate
   for the batch item

   -  ``framerate``: E.g. ‘24’ as string

-  ``setIncludeSourceCuePoints(includeSourceCuePoints: bool): bool`` : Toggle
   the inclusion of cue points [boolean] input value required

-  ``setOutputFrameSize(width: unsigned int, height: unsigned int): bool`` : Sets
   the output frame size

-  ``setRotation(rotationValue: float): bool`` : Sets the rotation
   (in a 360 degree system)

   -  ``rotationValue``: E.g. 0.0 - 360.0

-  ``setScaleType(scaleType: unsigned int): bool`` : Sets the scale
   type

   -  ``scaleType``: 0 ScaleToFit, 1 ScaleToFitBeforeCrop, 2
      SetAsOutputSize, 3 ScaleToFill, 4 ScaleToFillBeforeCrop, 5
      StretchToFill, 6 StretchToFillBeforeCrop

-  ``setTimeInterpolationType(interpolationType: unsigned int): bool`` : Set
   the time interpolation type

   -  ``interpolationType``: 0 FrameSampling, 1 FrameBlending, 2
      OpticalFlow

-  ``setUseFrameBlending(useFrameBlending: bool): bool`` : Toggle
   the use of frame blending [boolean] input value required

-  ``setUseMaximumRenderQuality(useMaximumRenderQuality: bool): bool`` : Toggle
   the use of maximum render quality [boolean] input value required

-  ``setUsePreviewFiles(usePreviewFiles: bool): bool`` : Toggle the
   use of previews files. [boolean] input value required

-  ``setWorkArea(workAreaType: unsigned int, startTime: float, endTime: float): bool`` : Sets
   the work area type, start and end time for the batch item

   -  ``workAreaType``: 0 Entire, 1 InToOut, 2 WorkArea, 3 Custom, 4
      UseDefault

-  ``setXMPData(templateXMPFilePath: string): bool`` : Sets XMP data
   to given template

EncoderWrapperEvent
-------------------

**An event to inform of encode progress and completion.**

.. _properties-6:

Properties
~~~~~~~~~~

-  ``onEncodeFinished: constant string`` : Notify when the batch
   item has been encoded.
-  ``onEncodeProgress: constant string`` : Notify when the batch
   item encode progress changes.
-  ``result: string`` : Returns the encoding result ‘Done!’,
   ‘Failed!’ or ‘Stopped!’ for the event type onEncodeFinished resp. the
   encoding progress for the event type onEncodeProgress which is
   between 0 and 100.

.. _code-samples-5:

Code Samples
~~~~~~~~~~~~

.. raw:: html

   <details>

   <summary>Example (click to expand):</summary>

.. code:: javascript

   var source = "D:\\full\\path\\to\\camera3.mxf";
   var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var destination = "C:\\full\\path\\to\\test";

   var exporter = app.getExporter();

   if (exporter) {
       var encoderWrapper  = exporter.exportItem(source, destination, preset);

       if (encoderWrapper) {
           encoderWrapper.addEventListener("onEncodeFinished", function(eventObj) {
           $.writeln("Encoding result: " + eventObj.result);
           }, false)

           encoderWrapper.addEventListener("onEncodeProgress", function(eventObj) {
               $.writeln("Encoding progress: " + eventObj.result);
           }, false)
       }
   }

.. raw:: html

   </details><br>

ExporterScriptObject
--------------------

**Contains several encoding methods. You can listen to different types
of the AMEExportEvent: onEncodeComplete, onError, onMediaInfoCreated,
onBatchItemStatusChanged, onItemEncodingStarted,
onEncodingItemProgressUpdated, onPostProcessListInitialized**

.. _properties-7:

Properties
~~~~~~~~~~

-  ``elapsedMilliseconds: float`` : Returns the encode time in
   milliseconds.
-  ``encodeID: string`` : Returns the current encode item ID as
   string.

.. _methods-3:

Methods
~~~~~~~

-  ``exportGroup(sourcePath: string, outputPath: string, presetsPath: string, matchSource: bool = false): bool`` : Export
   the source with the provided list of presets. Returns true in case of
   success.

   -  ``sourcePath``: Media path (Premiere Pro projects aren’t
      supported).
   -  ``outputPath``: If outputPath is empty, then the output file
      location will be generated based on the source location.
   -  ``presetsPath``: Multiple preset paths can be provided separated
      via a \| (e.g. ‘path1|path2|path3’
   -  ``matchSource``: Optional. Default value is false

-  ``exportItem(sourcePath: string, outputPath: string, presetPath: string, matchSource: bool = false, writeFramesToDisk: bool = false): scripting object`` : Export
   the source with the provided preset. Returns an EncoderWrapper
   object.

   -  ``sourcePath``: Media path or Premiere Pro project path (In case
      of a Premiere Pro project the last sequence will be used).
   -  ``outputPath``: If outputPath is empty, then the output file
      location will be generated based on the source location.
   -  ``matchSource``: Optional. Default value is false
   -  ``writeFramesToDisk``: Optional. Default value is false. True
      writes five frames at 0%, 25%, 50%, 75% and 100% of the full
      duration. Known issue: Currently it only works with parallel
      encoding disabled.

-  ``exportSequence(projectPath: string, outputPath: string, presetPath: string, matchSource: bool = false, writeFramesToDisk: bool = false, leadingFramesToTrim: int = 0, trailingFramesToTrim: int = 0, sequenceName: string = ""): bool`` : Export
   the Premiere Pro sequence with the provided preset. Returns true in
   case of success.

   -  ``projectPath``: Premiere Pro project path.
   -  ``outputPath``: If outputPath is empty, then the output file
      location will be generated based on the source location.
   -  ``matchSource``: Optional. Default value is false.
   -  ``writeFramesToDisk``: Optional. Default value is false. True
      writes five frames at 0%, 25%, 50%, 75% and 100% of the full
      duration. Known issue: Currently it only works with parallel
      encoding disabled.
   -  ``leadingFramesToTrim``: Optional. Default value is 0.
   -  ``trailingFramesToTrim``: Optional. Default value is 0.
   -  ``sequenceName``: Optional. If sequence name is empty then we use
      the last sequence of the project.

-  ``getSourceMediaInfo(sourcePath: string): scripting object`` : Returns
   a SourceMediaInfo object.

-  ``removeAllBatchItems(): bool`` : Remove all batch items from the
   queue. Returns true in case of success.

-  ``trimExportForSR(sourcePath: string, outputPath: string, presetPath: string, matchSource: bool = false, writeFramesToDisk: bool = false, leadingFramesToTrim: int = 0, trailingFramesToTrim: int = 0): bool`` : Smart
   render the source with the provided preset. Returns true in case of
   success.

   -  ``sourcePath``: Media path or Premiere Pro project path (In case
      of a Premiere Pro project the last sequence will be used).
   -  ``outputPath``: If outputPath is empty, then the output file
      location will be generated based on the source location.
   -  ``matchSource``: Optional. Default value is false.
   -  ``writeFramesToDisk``: Optional. Default value is false. True
      writes five frames at 0%, 25%, 50%, 75% and 100% of the full
      duration. Known issue: Currently it only works with parallel
      encoding disabled.
   -  ``leadingFramesToTrim``: Optional. Default value is 0.
   -  ``trailingFramesToTrim``: Optional. Default value is 0.

.. _code-samples-6:

Code Samples
~~~~~~~~~~~~

.. raw:: html

   <details>

   <summary>elapsedMilliseconds Example (click to expand):</summary>

.. code:: javascript

   var source = "D:\\full\\path\\to\\camera3.mxf";
   var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var destination = "C:\\full\\path\\to\\Output\\test";

   var exporter = app.getExporter();

   if (exporter) {
       var encoderWrapper  = exporter.exportItem(source, destination, preset);

       exporter.addEventListener("onEncodeComplete", function(eventObj) {
           // We can get the encoding time from the event or from the exporter
           $.writeln("Encode Complete Time (in milli seconds): " + eventObj.encodeCompleteTime);

           var encodeCompleteTime = exporter.elapsedMilliseconds;
           $.writeln("Encode Complete Time alt (in milli seconds): " + encodeCompleteTime);
       }, false)
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>encodeID Example (click to expand):</summary>

.. code:: javascript


   var exporter = app.getExporter();

   if (exporter) {
       var source = "D:\\full\\path\\to\\camera3.mxf";
       var preset = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
       var destination = "C:\\full\\path\\to\\Output\\test";
       var encoderWrapper = exporter.exportItem(source, destination, preset);
       // Currently the encodeID is fix for the exporter. This needs to be changed so that we create for every new encoding a unique encodeID.
       // The batch item will be created with the unique encodeID and shouldn't be identical for all encodings which comes from the exporter.
       var encodeID = exporter.encodeID;
       $.writeln("Encode ID: " + encodeID);
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>exportGroup Example (click to expand):</summary>

.. code:: javascript

   var source = "D:\\full\\path\\to\\camera3.mxf";
   var preset_1 = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 50i.epr";
   var preset_2 = "D:\\full\\path\\to\\AME\\MediaIO\\systempresets\\58444341_4d584658\\XDCAMHD 50 PAL 25p.epr";
   var presets = preset_1 + "|" + preset_2;
   var destination = "C:\\full\\path\\to\\Output\\test";
   var matchSourceSettings = false;  // optional

   var exporter = app.getExporter();

   if (exporter) { 

       var encodingPreperationSuccess = exporter.exportGroup(source, destination, presets, matchSourceSettings);
       // Without all optional arguments:
       // var encodingPreperationSuccess = exporter.exportGroup(source, destination, presets);

       $.writeln ("Encoding preparations were successful: " + encodingPreperationSuccess);
       
       if (encodingPreperationSuccess) {
           exporter.addEventListener("onEncodeComplete", function(eventObj) {
               // We should arrive here two times (for every preset we have one batch item)
               $.writeln("Encode Complete Status (always true): " + eventObj.encodeCompleteStatus);
               // We encode both batch items in parallel and so we don't really get the exact time for each batch item
               // When we arrive here the second time we get the total encoding time for both batch items (the first
               // could be ignored)
               $.writeln("Encode Complete Time (in milli seconds): " + eventObj.encodeCompleteTime);
           }, false)

           exporter.addEventListener("onError", function(eventObj) {
               $.writeln("Error while encoding");
           }, false)

           exporter.addEventListener("onBatchItemStatusChanged", function(eventObj) {
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
           }, false)

           exporter.addEventListener("oEncodingItemProgressUpdated", function(eventObj) {
               $.writeln("Encoding progress for batch item: " + eventObj.progress);
           }, false)

           exporter.addEventListener("onItemEncodingStarted", function(eventObj) {
               $.writeln("Encoding started for batch item.");
           }, false)

           exporter.addEventListener("onMediaInfoCreated", function(eventObj) {
               $.writeln("Media info created");
           }, false)
           
           exporter.addEventListener("onPostProcessListInitialized", function(eventObj) {
               $.writeln("Post process list has been initialized.");
           }, false)

           // also support old scripts without using event listeners (before dvascripting we used so called bindings)
           exporter.onEncodeComplete = function (status, time) {  
               $.writeln("Binding - Encode Complete Status (always true): " + status);    
               $.writeln("Binding - Complete Time (in milli seconds): " + time); 
           }
           exporter.onError = function () {  
               $.writeln("Binding - Error while encoding");
           }
       }
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>exportItem Example (click to expand):</summary>

.. code:: javascript

   try {
     var exporter = app.getExporter();
     result = exporter.exportItem(source, destination, preset, false, false);
     $.writeln("result ", result);
     if(result){
       $.writeln("file has been exported succesfully");
     }

   } catch(e) {
     $.writeln("something went wrong")
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>exportSequence Example (click to expand):</summary>

.. code:: javascript

   var projectPath = "C:\\full\\path\\to\\Validation.prproj";
   var preset = "C:\\full\\path\\to\\XDCAMHD50PAL25p.epr";
   var source = "C:\\full\\path\\to\\testmedia.mp4";
   var destination = "C:\\full\\path\\to\\Output";

   var matchSource= false;
   var writeFramesToDisk = false;
   var leadingFramesToTrim = 0;  // will be ignored => bug
   var trailingFramesToTrim = 4; // will be ignored => bug
   var sequenceName = "AME-Test-Sequence";

   var exporter = app.getExporter();

   if (exporter) { 

       var encodingPreperationSuccess = exporter.exportSequence(projectPath, destination, preset, matchSource, writeFramesToDisk, leadingFramesToTrim, trailingFramesToTrim, sequenceName);
      
       $.writeln ("Encoding preparations were successful: " + encodingPreperationSuccess);
    }

    

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>getSourceMediaInfo Example (click to expand):</summary>

.. code:: javascript

   var exporter = app.getExporter();

   if (exporter) {
       var source = "D:\\full\\path\\to\\camera3.mxf";
       var sourceMediaInfo = exporter.getSourceMediaInfo(source);
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>removeAllBatchItems Example (click to expand):</summary>

.. code:: javascript

   // Preparation: Be sure there are some batch items in the queue. Otherwise create them via scripting API's or directly in the UI
   // since we need some batch item in the queue to verify the API removeAllBatchItems

   var exporter = app.getExporter();

   if (exporter) {
       var success = exporter.removeAllBatchItems();
       $.writeln("Remove all batch items was successful: " + success);
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>trimExportForSR Example (click to expand):</summary>

.. code:: javascript

   var preset = "C:\\full\\path\\to\\DVAME-4199944\\XDCAMHD50PAL25p.epr";
   var source = "C:\\full\\path\\to\\testmedia.mp4";
   var destination = "C:full\\path\\to\\Output";
   var matchSource = false;
   var writeFramesToDisk = false;
   var leadingFramesToTrim = 10;  
   var trailingFramesToTrim = 700; 
   var exporter = app.getExporter();
   if (exporter) { 
       var encodingPreperationSuccess = exporter.trimExportForSR(source, destination, preset, matchSource, writeFramesToDisk, leadingFramesToTrim, trailingFramesToTrim);
      }

      

.. raw:: html

   </details><br>

FrontendScriptObject
--------------------

**Scripting methods to the frontend**

.. _methods-4:

Methods
~~~~~~~

-  ``addCompToBatch(compPath: string, presetPath: string = "", outputPath: string = ""): bool`` : Adds
   the first comp of an After Effects project resp. the first sequence
   of a Premiere Pro project to the batch.

   -  ``compPath``: Path to e.g. an After Effects project or Premiere
      Pro project. The first comp resp. sequence will be used.
   -  ``presetPath``: Optional. If presetPath is empty, then the default
      preset will be applied.
   -  ``outputPath``: Optional. If outputPath is empty, then the output
      file name will be generated based on the comp path.

-  ``addDLToBatch(projectPath: string, format: string, presetPath: string, guid: string, outputPath: string = ""): scripting object`` : Adds
   e.g. an After Effects comp or Premiere Pro sequence to the batch and
   returns an EncoderWrapper object.

   -  ``projectPath``: E.g. Premiere Pro or After Effects project path.
   -  ``format``: E.g. ‘H.264’
   -  ``presetPath``: Either a preset or a format input must be present.
      If no preset is used then the default preset of the specified
      format will be applied.
   -  ``guid``: The unique id of e.g. a Premiere Pro sequence or After
      Effects composition.
   -  ``outputPath``: Optional. If outputPath is empty, then the output
      file name will be generated based on the project path.

-  ``addFileSequenceToBatch(containingFolder: string, imagePath: string, presetPath: string, outputPath: string = ""): bool`` : Adds
   an image sequence to the batch. The images will be sorted in
   alphabetical order.

   -  ``containingFolder``: The folder containing image files.
   -  ``imagePath``: All images from the containing folder with the same
      extension will be added to the output file.
   -  ``outputPath``: Optional. If outputPath is empty, then the output
      file name will be generated based on the containingFolder name

-  ``addFileToBatch(filePath: string, format: string, presetPath: string, outputPath: string = ""): scripting object`` : Adds
   a file to the batch and returns an EncoderWrapper object.

   -  ``filePath``: File path of a media source.
   -  ``format``: E.g. ‘H.264’
   -  ``presetPath``: Either a preset or a format input must be present.
      If no preset is used then the default preset of the specified
      format will be applied.
   -  ``outputPath``: Optional. If outputPath is empty, then the output
      file name will be generated based on the file path.

-  ``addItemToBatch(sourcePath: string): bool`` : Adds a media
   source to the batch.

   -  ``sourcePath``: Path of the media source.

-  ``addTeamProjectsItemToBatch(projectsURL: string, format: string, presetPath: string, outputPath: string): scripting object`` : Adds
   a team project item to the batch and returns an EncoderWrapper
   object.

   -  ``projectsURL``: Team Projects URL or Team Projects Snap. You can
      create a tp2snap file in PPro for a ProjectItem via the scripting
      API saveProjectSnapshot.
   -  ``format``: E.g. ‘H.264’
   -  ``presetPath``: Either a preset or a format input must be present.
      If no preset is used then the default preset of the specified
      format will be applied.

-  ``addXMLToBatch(xmlPath: string, presetPath: string, outputFolderPath: string = ""): bool`` : Adds
   Final Cut Pro xml to the batch.

   -  ``xmlPath``: Path to a Final Cut Pro xml file.
   -  ``outputFolderPath``: Optional. If outputFolderPath is empty, then
      the output file name will be generated based on the XML file path.

-  ``getDLItemsAtRoot(projectPath: string): array of strings`` : Returns
   the list of GUIDs for objects (sequences/comps) at the top/root
   level.

   -  ``projectPath``: E.g. Premiere Pro or After Effects project path.

-  ``stitchFiles(mediaPaths: string, format: string, presetPath: string, outputPath: string): scripting object`` : Adds
   a batch item for the given media and returns an EncoderWrapper
   object.

   -  ``mediaPaths``: Semicolon delimited list of media paths.
   -  ``format``: E.g. ‘H.264’
   -  ``presetPath``: Either a preset or a format input must be present.
      If no preset is used then the default preset of the specified
      format will be applied.

-  ``stopBatch(): bool`` : Stops the batch.

SourceMediaInfo
---------------

**Get the width, height, PAR, duration, etc about the imported source**

.. _properties-8:

Properties
~~~~~~~~~~

-  ``audioDuration: string`` : Returns the audio duration of the
   source
-  ``description: string`` : Returns embedded description of the
   source
-  ``dropFrameTimeCode: bool`` : Returns true if the timecode is a
   drop frame timecode
-  ``duration: string`` : Returns duration of the source
-  ``fieldType: string`` : Returns field type of the source
-  ``frameRate: string`` : Returns frame rate of the source
-  ``height: string`` : Returns height of the source
-  ``importer: string`` : Returns the importer used to decode the
   source
-  ``numChannels: string`` : Returns the number of audio channels of
   the source
-  ``parX: string`` : Returns the X PAR of the source
-  ``parY: string`` : Returns the Y PAR of the source
-  ``sampleRate: string`` : Returns sample rate of the source
-  ``width: string`` : Returns width of the source
-  ``xmp: string`` : Returns xmp xml of the source

.. _code-samples-7:

Code Samples
~~~~~~~~~~~~

.. raw:: html

   <details>

   <summary>Example (click to expand):</summary>

.. code:: javascript

   var source = "D:\\full\\path\\to\\camera3.mxf";

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

.. raw:: html

   </details><br>

WatchFolderScriptEvent
----------------------

**An event to inform of batch item import completion**

.. _properties-9:

Properties
~~~~~~~~~~

-  ``elapsedTime: float`` : Returns the encoding elapsed time in
   milliseconds.
-  ``onEncodeComplete: constant string`` : Notify when the
   watchfolder job item is complete
-  ``onEncodeError: constant string`` : Notify when the watchfolder
   job encode fails

WatchFolderScriptObject
-----------------------

**Scripting methods to watch folders**

.. _methods-5:

Methods
~~~~~~~

-  ``createWatchFolder(folderPath: string, outputPath: string, presetPath: string): bool`` : Create
   a watch folder at destination for the preset and add the source

   -  ``folderPath``: The path to the folder which should be added as
      watch folder

-  ``removeAllWatchFolders(): bool`` : Remove all watch folders

.. _code-samples-8:

Code Samples
~~~~~~~~~~~~

.. raw:: html

   <details>

   <summary>createWatchFolder Example (click to expand):</summary>

.. code:: javascript

   var folder = "C:\\full\\path\\to\\watchFolder";
   var preset = "C:\\full\\path\\to\\HighQuality720HD.epr";
   var destination = "C:\\full\\path\\to\\watchFolder";

   try {
     var watchFolder = app.getWatchFolder();
     if (watchFolder) {
       var watchFolderSuccess = watchFolder.createWatchFolder(
         folder,
         destination,
         preset
       );

       if (watchFolderSuccess) {
         var date = Date();
         $.writeln(
           "WatchfolderScriptObject script engine added the folder to watch: \n",
           folder,
           "\t",
           date
         );

         // this is a global object that sends an oncomplete signal once the whole queue is completed
         encoderHostWrapper = app.getEncoderHost();

         if (encoderHostWrapper) {
           encoderHostWrapper.addEventListener(
             "onItemEncodeComplete",
             function (eventObj) {
               $.writeln("encoderHostWrapper.onItemEncodeComplete: success");
             }
           );

           watchFolder.addEventListener("onEncodeComplete", function (eventObj) {
             $.writeln("Elapsed Time: " + eventObj.elapsedTime);
             $.writeln("watchFolder.onEncodeComplete: success");
           });

           watchFolder.addEventListener("onEncodeError", function (eventObj) {
             $.writeln("watchFolder.onEncodeError: failed");
           });

           // Test for old world scripts which used bindings:
           watchFolder.onEncodeComplete = function (eventObj) {
             $.writeln("Elapsed Time: " + eventObj.elapsedTime);
           };

           encoderHostWrapper.runBatch();

         } else {
           throw "The folder path must be set to a folder";
         }
       }
     }
   } catch (e) {
     $.writeln(e);
   }

.. raw:: html

   </details><br>

.. raw:: html

   <details>

   <summary>removeAllWatchFolders Example (click to expand):</summary>

.. code:: javascript

   var folder = "C:\\full\\path\\to\\watchFolder";
   var folder1 = "C:\\full\\path\\to\\watchFolder1";
   var folder2 = "C:\\full\\path\\to\\watchFolder2";
   var preset = "C:\\full\\path\\to\\HighQuality720HD.epr";
   var destination = "C:\\full\\path\\to\\watchFolder";

   function addWatchFolder(path, watchFolder) {
     if (watchFolder) {
       var watchFolderSuccess = watchFolder.createWatchFolder(
         path,
         destination,
         preset
       );

       if (watchFolderSuccess) {
         var date = Date();
         $.writeln(
           "WatchfolderScriptObject script engine added the folder to watch: \n",
           path,
           "\t",
           date
         );
       } else {
         throw "The folder path must be set to a folder";
       }
     }
   }

   try {
     var obj = app.getWatchFolder();
     if (obj) {
       addWatchFolder(folder, obj);
       addWatchFolder(folder1, obj);
       //addWatchFolder(folder2, obj);

       obj.removeAllWatchFolders();
     }
   } catch (e) {
     $.writeln(e);
   }

.. raw:: html

   </details><br>
