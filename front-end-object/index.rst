.. _front-end-object:

Front End Object
=================

The Front End Object allows you to add items to the render queue (aka batch).

``app.getFrontend()``

Methods
-------


FrontEnd.addDLToBatch()
****************************************************************
Add Comp from After Effects or Sequences from Premiere Pro.

``app.getFrontend().addDLToBatch(project, format, preset, guid, destination)``

**Parameters**

===================   ============================================================================
``project``           Path to the project file. (e.g. .aep, .prpro)
``format``            The name of the format used in the preset EPR. (e.g. H.264, Quicktime, etc.)
``preset``            Path to the EPR preset file.
``guid``              The Dynamic Link GUID from the After Effects Comp or Premiere Pro Sequence
``destination``       Path to the destination file.
===================   ============================================================================

**Returns**

Batch Item Object

*Note: Application will hang until process is complete or will throw an exception if arguments
are incorrect. This method ignores work area. This method resets timecode to 00:00:00.*



FrontEnd.addCompToBatch()
****************************************************************
Add Comp from After Effects. 

``app.getFrontend().addCompToBatch(project, preset, destination)``

**Parameters**

===================   ============================================================================
``project``           Path to the project file. (e.g. .aep, .prpro)
``preset``            Path to the EPR preset file.
``destination``       Path to the destination file.
===================   ============================================================================


**Returns**

Batch Item Object

*Note: Method requires project to be structured so that 1 and only 1 comp is at the root of
the project. Application will throw an exception if no comp is present in the root or more 
than 1 comps are present.*



FrontEnd.addItemToBatch()
****************************************************************
Add any footage item into the render queue.

``app.getFrontend().addItemToBatch(file)``

**Parameters**

===================   ============================================================================
``file``              Path to the media file.
===================   ============================================================================

**Returns**

Batch Item Object



FrontEnd.addFileToBatch()
****************************************************************
Add any footage item into the render queue.

``app.getFrontend().addFileToBatch(file, format, preset, destination)``

**Parameters**

===================   ============================================================================
``file``              Path to the media file.
``format``            The name of the format used in the preset EPR. (e.g. H.264, Quicktime, etc.)
``preset``            Path to the EPR preset file.
``destination``       Path to the destination file.
===================   ============================================================================

**Returns**

Batch Item Object


FrontEnd.addFileSequenceToBatch()*
****************************************************************
Add any footage item into the render queue.

``app.getFrontend().addFileSequenceToBatch()`` *\*untested*

**Parameters**

*unknown*

**Returns**

Batch Item Object


FrontEnd.addXMLToBatch()*
****************************************************************
Add an XML project to the batch.

``app.getFrontend().addXMLToBatch()`` *\*untested*

**Parameters**

*unknown*

**Returns**

Batch Item Object


FrontEnd.addTeamProjectsItemToBatch()*
****************************************************************
Add team project items to the render queue.

``app.getFrontend().addTeamProjectsItemToBatch()`` *\*untested*

**Parameters**

*unknown*

**Returns**

Batch Item Object


FrontEnd.stitchFiles()*
****************************************************************
Stitch files for rendering.

``app.getFrontend().stitchFiles()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


FrontEnd.stopBatch()*
****************************************************************
Stops batch from running. 

``app.getFrontend().stopBatch()``

**Parameters**

None.

**Returns**

Result Boolean.
