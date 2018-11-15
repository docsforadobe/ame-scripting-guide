.. _encoder-host-object:

Encoder Host Object
===================

The Encoder Host Object allows you to control and query the render queue.

``app.getEncoderHost()``

Methods
-------


Run Batch
*********
Starts the render queue.

``app.getEncoderHost().runBatch()``

**Parameters**

None.

**Returns**

Result Boolean.


Pause Batch
***********
Pauses the render queue.

``app.getEncoderHost().pauseBatch()``

**Parameters**

None.

**Returns**

Result Boolean.


Stop Batch
**************************
Stops the render queue.

``app.getEncoderHost().stopBatch()``

**Parameters**

None.

**Returns**

Result Boolean.


Is Batch Running
****************
Checks if the render queue is rendering.

``app.getEncoderHost().isBatchRunning()``

**Parameters**

None.

**Returns**

Result Boolean.


Get Format List
****************
Checks if the render queue is rendering.

``app.getEncoderHost().getFormatList()``

**Parameters**

None.

**Returns**

Array of Format Names as Strings.


Create Encoder for Format*
**************************
Creates an Encoder for a given Format

``app.getEncoderHost().createEncoderForFormat(format)`` *\*untested*

**Parameters**

format: String. format name (H.264, Quicktime, etc.)

**Returns**

Encoder Object. (encode from there with ``encoder.encode(src,dst)``)


Create Media Comparator*
************************

``app.getEncoderHost().createMediaComparator()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Get Supported Import File Types*
********************************

``app.getEncoderHost().getSupportedImportFileTypes()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Get Source Info*
****************

``app.getEncoderHost().getSourceInfo()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Get Current Batch Preview Info*
*******************************

``app.getEncoderHost().getCurrentBatchPreview()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*
