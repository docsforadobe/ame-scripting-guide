.. _batch-item-object:

Batch Item Object
=================

The Batch Item Object is returned after adding an item to the render queue, and then
allows you to make further modifications and track the status of the render job.

:ref:`More on adding items to the queue with Front End Object <front-end-object>`

Attributes
----------


BatchItem.outputFiles
************************************************************

``batchItem.outputFiles``

Contains the output files from the render job once the job is complete.

**Type**

Array; read-only.


BatchItem.encodeProgress
************************************************************

``batchItem.encodeProgress``

The encode progress from 0 - 100 of render job.

**Type**

Number; read-only.


BatchItem.outputWidth*
************************************************************

``batchItem.outputWidth`` *\*untested*

*unknown*

**Type**

Number;


BatchItem.outputHeight*
************************************************************

``batchItem.outputHeight`` *\*untested*

*unknown*

**Type**

Number;



Methods
-------


BatchItem.getEncodeProgress()
************************************************************
Gets the progress from 0 - 100 of the render job.

``batchItem.getEncodeProgress()``


**Parameters**

None.

**Returns**

Number.


BatchItem.getEncodeTime()*
************************************************************

``batchItem.getEncodeTime()`` *\*untested*

**Parameters**

None.

**Returns**

Number.


BatchItem.getCurrentBatchPreview()*
************************************************************

``batchItem.getCurrentBatchPreview()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.getPresetList()*
************************************************************

``batchItem.getPresetList()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.getMissingAssets()*
************************************************************

``batchItem.getMissingAssets()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.loadPreset()*
************************************************************

``batchItem.loadPreset()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.loadFormat()*
************************************************************

``batchItem.loadFormat()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.setWorkArea()*
************************************************************

``batchItem.setWorkArea()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.setUsePreviewFiles()*
************************************************************

``batchItem.setUsePreviewFiles()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.setUseMaximumRenderQuality()*
************************************************************

``batchItem.setUseMaximumRenderQuality()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.setUseFrameBlending()*
************************************************************

``batchItem.setUseFrameBlending()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.setIncludeSourceXMP()*
************************************************************

``batchItem.setIncludeSourceXMP()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.setIncludeSourceCuePoints()*
************************************************************

``batchItem.setIncludeSourceCuePoints()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.setCropState()*
************************************************************

``batchItem.setCropState()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.setCropType()*
************************************************************

``batchItem.setCropType()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.setCropOffsets()*
************************************************************

``batchItem.setCropOffsets()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.setOutputFrameSize()*
************************************************************

``batchItem.setOutputFrameSize()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.setXMPData()*
************************************************************

``batchItem.setXMPData()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.setCuePointData()*
************************************************************

``batchItem.setCuePointData()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


BatchItem.setScaleType()*
************************************************************

``batchItem.setScaleType()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*
