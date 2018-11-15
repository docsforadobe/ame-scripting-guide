.. _exporter-object:

Exporter Object
=================

The Exporter Object allows you to add items to the render queue.

``app.getExporter()``

Attributes
----------


Encode ID
**********

``app.getExporter().encodeID``

Gets the current Encode ID.

**Type**

String; read-only.


Result
******

``app.getExporter().result``

*unknown*

**Type**

Boolean; read-only.


Elapsed Milliseconds
********************

``app.getExporter().elapsedMilliseconds``

Elapsed milliseconds since queue started.

**Type**

Number; read-only.


Methods
-------

Add Dynamic Link to Batch
*************************
Add Comps from After Effects or Sequences from Premiere Pro.

``app.getExporter().removeAllBatchItems()``

**Parameters**

None.

**Returns**

Result Boolean.

Note: Application will hang until process is complete.


Trim Export for SR*
*******************

``app.getExporter().trimExportForSR()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Export Item*
************

``app.getExporter().ExportItem()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Export Group*
*************

``app.getExporter().ExportGroup()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*
