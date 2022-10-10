.. _introduction:

Introduction
============

This is a community-created and -maintained project documenting the Extendscript scripting API for Adobe Media Encoder.


How do I run scripts in AME?
----------------------------

Similarly to Premiere Pro, you can use Visual Studio Code and the "ExtendScript Debugger" extension to send scripts from VS Code to AME and debug them in the app. Open the script, select the target and run or debug the script.

You can also launch scripts from the command line on Mac and Windows, like this

.. code:: bash

   <fullPathToAMEbinary> --console es.processFile <fullPathToScript>


Example for executing a test script on Mac:

.. code:: bash

   "/Applications/Adobe Media Encoder (Beta)/Adobe Media Encoder (Beta).app/Contents/MacOS/Adobe Media Encoder (Beta)" --console es.processFile /full/path/to/test.js


