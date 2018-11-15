.. _exporter-object:

Exporter Object
=================

The Exporter Object allows you to access and modify the render queue.

``app.getExporter()``

Attributes
----------


Exporter.encodeID
****************************************************************

``app.getExporter().encodeID``

Gets the current Encode ID.

**Type**

String; read-only.


Exporter.result
****************************************************************

``app.getExporter().result``

*unknown*

**Type**

Boolean; read-only.


Exporter.elapsedMilliseconds
****************************************************************

``app.getExporter().elapsedMilliseconds``

Elapsed milliseconds since queue started.

**Type**

Number; read-only.


Methods
-------

Exporter.removeAllBatchItems()
****************************************************************
Removes all items from the render queue.

``app.getExporter().removeAllBatchItems()``

**Parameters**

None.

**Returns**

Result Boolean.

*Note: Application will hang until process is complete.*


Exporter.trimExportForSR()*
****************************************************************

``app.getExporter().trimExportForSR()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Exporter.ExportItem()*
****************************************************************

``app.getExporter().exportItem()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


Exporter.ExportGroup()*
****************************************************************

``app.getExporter().exportGroup()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*
