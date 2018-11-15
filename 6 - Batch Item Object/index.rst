.. _batch-item-object:

Batch Item Object
=================

The Batch Item Object is returned after adding an item to the render queue, and then
allows you to make further modifications and track the status of the render job.

:ref:`More on adding items to the queue with Front End Object <front-end-object>`

Attributes
----------


Output Files
************

``batchItem.outputFiles``

Contains the output files from the render job once the job is complete.

**Type**

Array; read-only.


Encode Progress
***************

``batchItem.encodeProgress``

The encode progress from 0 - 100 of render job.

**Type**

Number; read-only.


Output Width*
*************

``batchItem.outputWidth`` *\*untested*

*unknown*

**Type**

Number;


Output Height*
**************

``batchItem.outputHeight`` *\*untested*

*unknown*

**Type**

Number;



Methods
-------


Get Encode Progress
*******************
Gets the progress from 0 - 100 of the render job.

``batchItem.getEncodeProgress()``


**Parameters**

None.

**Returns**

Number.


Get Encode Time*
****************

``batchItem.getEncodeTime()`` *\*untested*

**Parameters**

None.

**Returns**

Number.


Get Current Batch Preview Info*
*******************************

``batchItem.getCurrentBatchPreview()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Get Preset List*
****************

``batchItem.getPresetList()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Get Missing Assets*
*******************

``batchItem.getMissingAssets()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Load Preset*
************

``batchItem.loadPreset()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Load Format*
************

``batchItem.loadFormat()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Set Work Area*
**************

``batchItem.setWorkArea()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Set Use Preview Files*
**********************

``batchItem.setUsePreviewFiles()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Set Use Maximum Render Quality*
*******************************

``batchItem.setUseMaximumRenderQuality()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Set Use Frame Blending*
***********************

``batchItem.setUseFrameBlending()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Set Include Source XMP*
***********************

``batchItem.setIncludeSourceXMP()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Set Include Source Cue Points*
******************************

``batchItem.setIncludeSourceCuePoints()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Set Crop State*
***************

``batchItem.setCropState()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Set Crop Type*
**************

``batchItem.setCropType()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Set Crop Offsets*
*****************

``batchItem.setCropOffsets()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Set Output Frame Size*
**********************

``batchItem.setOutputFrameSize()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Set XMP Data*
*************

``batchItem.setXMPData()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Set Cue Point Data*
*******************

``batchItem.setCuePointData()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Set Scale Type*
***************

``batchItem.setScaleType()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*
