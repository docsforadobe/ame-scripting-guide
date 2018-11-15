.. _application-object:

Application Object
==================

The Application Object is the master object for controlling the application.

``app``

Attributes
----------

app.launchTime
****************************************************************

``app.launchTime``

The time in seconds AME took to launch.

**Type**

Number; read-only.


Methods
-------


app.getFrontend()
****************************************************************
:ref:`More on the Front End Object <front-end-object>`

``app.getFrontend()``

**Parameters**

None.

**Returns**

Front End Object.


app.getExporter()
****************************************************************
:ref:`More on the Exporter Object <exporter-object>`

``app.getExporter()``

**Parameters**

None.

**Returns**

Exporter Object.


app.getEncoderHost()
****************************************************************
:ref:`More on the Encoder Host Object <encoder-host-object>`

``app.getEncoderHost()``

**Parameters**

None.

**Returns**

Encoder Host Object.


app.quit()
****************************************************************
Quits the application.

``app.quit()``

**Parameters**

None.

**Returns**

None.


app.getBuildNumber()
****************************************************************
Gets the application build number.

``app.getBuildNumber()``

**Parameters**

None.

**Returns**

String. Build number.

.. _app.scheduleTask:

app.scheduleTask()
****************************************************************
Schedule a task for a later time. (similar to ``setTimeout()`` in modern JS)

``app.scheduleTask()``

**Parameters**

===================   ==============================================
``stringToExecute``   A string containing JavaScript to be executed.
``delay``             A number of milliseconds to wait before executing
                      the JavaScript. A floating-point value.
``repeat``            When true, execute the script repeatedly, with the
                      specified delay between each execution. When false the
                      script is executed only once.
===================   ==============================================

**Returns**

Integer, a unique identifier for this task, which can be used to cancel it with `app.cancelTask()`_.

.. _app.cancelTask:

app.cancelTask()
****************************************************************
Removes the specified task from the queue of tasks scheduled for delayed execution.

``app.cancelTask(taskID)``

**Parameters**

==========  =============================
``taskID``  An integer that identifies the task, as returned by
            `app.scheduleTask()`_.
==========  =============================

**Returns**

Nothing.


app.getWatchFolder()*
****************************************************************

``app.getWatchFolder()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*



app.wait()*
****************************************************************

``app.wait()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


app.assertToConsole()*
****************************************************************

``app.assertToConsole()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


app.renderFrameSequence()*
****************************************************************

``app.renderFrameSequence()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


app.isBlackVideo()*
****************************************************************

``app.isBlackVideo()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


app.isSilentAudio()*
****************************************************************

``app.isSilentAudio()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*


app.simulateUIEvents()*
****************************************************************

``app.simulateUIEvents()`` *\*untested*

**Parameters**

*unknown*

**Returns**

*unknown*
