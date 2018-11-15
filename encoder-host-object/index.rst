.. _encoder-host-object:

Encoder Host Object
===================

The Encoder Host Object allows you to control and query the render queue.

``app.getEncoderHost()``

Methods
-------


EncoderHost.runBatch()
****************************************************************
Starts the render queue.

``app.getEncoderHost().runBatch()``

**Parameters**

None.

**Returns**

Result Boolean.


EncoderHost.pauseBatch()
****************************************************************
Pauses the render queue.

``app.getEncoderHost().pauseBatch()``

**Parameters**

None.

**Returns**

Result Boolean.


EncoderHost.stopBatch()
****************************************************************
Stops the render queue.

``app.getEncoderHost().stopBatch()``

**Parameters**

None.

**Returns**

Result Boolean.


EncoderHost.isBatchRunning()
****************************************************************
Checks if the render queue is rendering.

``app.getEncoderHost().isBatchRunning()``

**Parameters**

None.

**Returns**

Result Boolean.


EncoderHost.getFormatList()
****************************************************************
Checks if the render queue is rendering.

``app.getEncoderHost().getFormatList()``

**Parameters**

None.

**Returns**

Array of Format Names as Strings.


EncoderHost.createEncoderForFormat()
****************************************************************
Creates an Encoder for a given Format

``app.getEncoderHost().createEncoderForFormat(format)`` *\*untested*

**Parameters**

format: String. format name (H.264, Quicktime, etc.)

**Returns**

Encoder Object. (encode from there with ``encoder.encode(src,dst)``)


EncoderHost.createMediaComparator()*
****************************************************************

``app.getEncoderHost().createMediaComparator()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


EncoderHost.getSupportedImportFileTypes()*
****************************************************************

``app.getEncoderHost().getSupportedImportFileTypes()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


EncoderHost.getSourceInfo()*
****************************************************************

``app.getEncoderHost().getSourceInfo()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


EncoderHost.getCurrentBatchPreview()*
****************************************************************

``app.getEncoderHost().getCurrentBatchPreview()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*
