.. _introduction:

AME Scripting Guide
===================

This is a community-created and -maintained project documenting Extendscript usage for Adobe Media Encoder.


How do I run scripts in AME?
----------------------------

Similarly to Premiere Pro, you can use Visual Studio Code and the "ExtendScript Debugger" extension to send scripts from VS Code to AME and debug them in the app. Open the script, select the target and run or debug the script.

You can also launch scripts from the command line on Mac and Windows, like this

.. code:: bash

   <fullPathToAMEbinary> --console es.processFile <fullPathToScript>


Example for executing a test script on Mac:

.. code:: bash

   "/Applications/Adobe Media Encoder (Beta)/Adobe Media Encoder (Beta).app/Contents/MacOS/Adobe Media Encoder (Beta)" --console es.processFile /full/path/to/test.js


What is a good starting point to understand the scripting objects?
------------------------------------------------------------------

Let's start with a very basic script:

.. code:: javascript

   var source = "C:\\full\\path\\to\\testmedia.mp4";
   var preset = "C:\\full\\path\\to\\HighQuality720HD.epr";
   var destination = "C:\\full\\path\\to\\Output";

   try {
     var exporter = app.getExporter();
     result = exporter.exportItem(source, destination, preset, false, false);
     $.writeln("result ", result);
     if (result){
       $.writeln("file has been exported succesfully");
     }
   } catch (e) {
     $.writeln("something went wrong")
   }

In order to encode a source file in AME, you need to provide the paths of the source file and destination folder, and the preset to be used.

Where can I ask more questions and get help?
--------------------------------------------

Got more questions than what is covered here? Head over to the Adobe Media Encoder community here:

https://community.adobe.com/t5/adobe-media-encoder/ct-p/ct-media-encoder