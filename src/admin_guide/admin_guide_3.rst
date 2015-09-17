.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2015 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    LeoFS Storage operation

LeoFS Storage Operation
=======================

.. index::
   pair: LeoFS Storage cluster; LeoFS Storage operation

LeoFS Storage's Cluster Operation
---------------------------------

* LeoFS Storage operation commands are executed on **LeoFS-Manager console** OR the ``leofs-adm`` script.
* Refer :ref:`LeoFS operation flow diagram <operation-flow-diagram-label>`

+-----------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| **Shell**                                                 | **Description**                                                                                   |
+===========================================================+===================================================================================================+
| leofs-adm :ref:`detach <detach-command>` <storage-node>   | * Remove the storage node from the LeoFS storage cluster                                          |
|                                                           | * Current status: ``running`` | ``stop``                                                          |
+-----------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`suspend <suspend-command>` <storage-node> | * Suspend a storage node for maintenance                                                          |
|                                                           | * This command is NOT similar to the detach command, just only to suspend the node.               |
|                                                           | * While suspending, it rejects any requests                                                       |
|                                                           | * Current status: ``running``                                                                     |
+-----------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`resume <resume-command>` <storage-node>   | * Resume a storage node until a finished maintenance                                              |
|                                                           | * Current status: ``suspended`` | ``restarted``                                                   |
+-----------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`start <start-command>`                    | * Start LeoFS after distributing the RING from LeoFS Manager to LeoFS Storage and Gateway         |
+-----------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`rebalance <rebalance-command>`            | * Commit detached and attached nodes to update the cluster and Ring(routing-table)                |
|                                                           | * Rebalance objects in the cluster which is based on the updated cluster topology                 |
+-----------------------------------------------------------+---------------------------------------------------------------------------------------------------+


.. index::
    pair: Storage operation; detach-command

.. _detach-command:

detach <storage-node>
^^^^^^^^^^^^^^^^^^^^^

Remove the storage node from the storage cluster
"""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: bash

    $ leofs-adm detach storage_0@127.0.0.1
    OK
    $ leofs-adm rebalance
    OK


Method of cancellation of detach-operation
"""""""""""""""""""""""""""""""""""""""""""

.. code-block:: bash

    $ leofs-adm detach storage_0@127.0.0.1
    OK
    ### Rollback this operation
    $ leofs-adm rollback storage_0@127.0.0.1
    OK

Method of deletion of an attached node
"""""""""""""""""""""""""""""""""""""""

.. code-block:: bash

    ## Confirm current status of ther storage cluster
    $ leofs-adm status
     [System Confiuration]
    -----------------------------------+----------
     Item                              | Value
    -----------------------------------+----------
     Basic/Consistency level
    -----------------------------------+----------
                        system version | 1.2.12
                            cluster Id | leofs_1
                                 DC Id | dc_1
                        Total replicas | 2
              number of successes of R | 1
              number of successes of W | 1
              number of successes of D | 1
     number of rack-awareness replicas | 0
                             ring size | 2^128
    -----------------------------------+----------
     Multi DC replication settings
    -----------------------------------+----------
            max number of joinable DCs | 2
               number of replicas a DC | 1
    -----------------------------------+----------
     Manager RING hash
    -----------------------------------+----------
                     current ring-hash | 0364701d
                    previous ring-hash | 0364701d
    -----------------------------------+----------

     [State of Node(s)]
    -------+--------------------------+--------------+----------------+----------------+----------------------------
     type  |           node           |    state     |  current ring  |   prev ring    |          updated at
    -------+--------------------------+--------------+----------------+----------------+----------------------------
      S    | storage_0@127.0.0.1      | attached     |                |                | 2015-09-09 11:17:49 +0900
      S    | storage_1@127.0.0.1      | running      | 0364701d       | 0364701d       | 2015-09-09 11:13:31 +0900
      S    | storage_2@127.0.0.1      | running      | 0364701d       | 0364701d       | 2015-09-09 11:09:15 +0900
      G    | gateway_0@127.0.0.1      | running      | 0364701d       | 0364701d       | 2015-09-09 10:53:19 +0900
    -------+--------------------------+--------------+----------------+----------------+----------------------------

    ## Remove an attached node
    $ leofs-adm detach storage_0@127.0.0.1
    OK

    ## Confirm the current status of the storage cluster, again
    $ leofs-adm status
     [System Confiuration]
    -----------------------------------+----------
     Item                              | Value
    -----------------------------------+----------
     Basic/Consistency level
    -----------------------------------+----------
                        system version | 1.2.12
                            cluster Id | leofs_1
                                 DC Id | dc_1
                        Total replicas | 2
              number of successes of R | 1
              number of successes of W | 1
              number of successes of D | 1
     number of rack-awareness replicas | 0
                             ring size | 2^128
    -----------------------------------+----------
     Multi DC replication settings
    -----------------------------------+----------
            max number of joinable DCs | 2
               number of replicas a DC | 1
    -----------------------------------+----------
     Manager RING hash
    -----------------------------------+----------
                     current ring-hash | 0364701d
                    previous ring-hash | 0364701d
    -----------------------------------+----------

     [State of Node(s)]
    -------+--------------------------+--------------+----------------+----------------+----------------------------
     type  |           node           |    state     |  current ring  |   prev ring    |          updated at
    -------+--------------------------+--------------+----------------+----------------+----------------------------
      S    | storage_1@127.0.0.1      | running      | 0364701d       | 0364701d       | 2015-09-09 11:13:31 +0900
      S    | storage_2@127.0.0.1      | running      | 0364701d       | 0364701d       | 2015-09-09 11:09:15 +0900
      G    | gateway_0@127.0.0.1      | running      | 0364701d       | 0364701d       | 2015-09-09 10:53:19 +0900

.. index::
    pair: Storage operation; suspend-command

.. _suspend-command:

suspend <storage-node>
^^^^^^^^^^^^^^^^^^^^^^

* Suspend the storage node
* While suspending, it rejects any requests
* This command does NOT detach the node from the storage cluster

.. code-block:: bash

    $ leofs-adm suspend storage_0@127.0.0.1
    OK


.. index::
    pair: Storage operation; resume-command

.. _resume-command:

resume <storage-node>
^^^^^^^^^^^^^^^^^^^^^

* Resume the storage node

.. code-block:: bash

    $ leofs-adm resume storage_0@127.0.0.1
    OK

\


.. index::
    pair: Storage operation; start-command

.. _start-command:

start
^^^^^

* Start LeoFS after distributing the RING from LeoFS Manager to LeoFS Storage and LeoFS Gateway

.. code-block:: bash

    $ leofs-adm start
    OK

\


.. index::
    pair: Storage operation; rebalance-command

.. _rebalance-command:

rebalance
^^^^^^^^^

* Commit detached and attached node(s) to update the cluster and Ring(routing-table)
* Rebalance objects in the cluster which is based on the updated cluster topology

.. code-block:: bash

    $ leofs-adm rebalance
    OK

\

.. index::
   pair: LeoFS Storage MQ; LeoFS Storage operation

Storage MQ Operation
--------------------

Since
^^^^^^^^^

LeoFS v1.2.2

Overview
^^^^^^^^^

LeoFS Storage MQ is controllable mechanism manually. We've published ``mq-suspend`` and ``mq-resume`` command in ``leofs-adm`` script.
In addition, LeoFS's MQ mechanism is affected by ``the watchdog mechanism`` to reduce comsumption of message costs.

Description of the each MQ
""""""""""""""""""""""""""

+---------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| Id                              | Description                                                                                                                                             |
+=================================+=========================================================================================================================================================+
| leo_delete_dir_queue            | After executed ``leofs-adm delete-bucket``, messages of deletion object is added into the queue.                                                        |
+---------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| leo_comp_meta_with_dc_queue     | After executed ``leofs-adm recover-cluster``, messages of comparison of metadata w/remote-node is added into the queue.                                 |
+---------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| leo_sync_obj_with_dc_queue      | After executed ``leofs-adm recover-cluster``, messages of synchronization of objects w/remote-node is added into the queue.                             |
+---------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| leo_recovery_node_queue         | After executed ``leofs-adm recover-node``, messages of recovery objects of a node is added into the queue.                                              |
+---------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| leo_async_deletion_queue        | After executed ``leofs-adm delete-bucket`` OR deletion of a bucket OR deletion of an object, message of async deletion of objs is added into the queue. |
+---------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| leo_rebalance_queue             | After executed ``leofs-adm rebalance``, messages of rebalance is added in to the queue.                                                                 |
+---------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| leo_sync_by_vnode_id_queue      | After executed ``leofs-adm rebalance``, messages of synchronization of virtual-nodes is added into the queue.                                           |
+---------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| leo_per_object_queue            | After executed ``leofs-adm rebalance`` OR ``leofs-adm recover-file`` OR ``leofs-adm recover-node``                                                      |
|                                 | OR fixing inconsistent object(s) with the recovery data mechanism, messages of recover inconsistent objects is added into the queue.                    |
+---------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+


Commands
""""""""

+--------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| **Shell**                                                                | **Description**                                                                                   |
+==========================================================================+===================================================================================================+
| leofs-adm :ref:`mq-stats <mq-stats-command>` <storage-node>              | * See the statuses of message queues used in LeoFS Storage                                        |
+--------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`mq-suspend <mq-suspend-command>` <storage-node> <mq-id>  | * Suspend a process consuming a message queue                                                     |
|                                                                          | * Active message queues only can be suspended                                                     |
|                                                                          | * While suspending, no messages are consumed                                                      |
+--------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`mq-resume <mq-resume-command>` <storage-node> <mq-id>    | * Resume a process consuming a message queue                                                      |
+--------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------+

\

.. _mq-stats-command:

mq-stats <storage-node>
^^^^^^^^^^^^^^^^^^^^^^^

You can check tatuses of the message queues in the LeoFS's storage node.
\
Explanation of columns:

+-----------------+------------------------------------------------------------------------------------------------+
| **Column**      | **Description**                                                                                |
+=================+================================================================================================+
| state           | A status of the MQ - [idling, running, suspending]                                             |
+-----------------+------------------------------------------------------------------------------------------------+
| number of msgs  | A number of messages in the queue                                                              |
+-----------------+------------------------------------------------------------------------------------------------+
| batch of msgs   | A batch of messages of the MQ's message-consumption                                            |
+-----------------+------------------------------------------------------------------------------------------------+
| interval        | An interval time between the batch processing                                                  |
+-----------------+------------------------------------------------------------------------------------------------+

.. code-block:: bash

    $ ./leofs-adm mq-stats storage_0@127.0.0.1
                  id                |    state    | number of msgs | batch of msgs  |    interval    |            description
    --------------------------------+-------------+----------------|----------------|----------------|-----------------------------------
    leo_delete_dir_queue            |   idling    | 0              | 1000           | 100            | delete directories
    leo_comp_meta_with_dc_queue     |   idling    | 0              | 1000           | 100            | compare metadata w/remote-node
    leo_sync_obj_with_dc_queue      |   idling    | 0              | 1000           | 100            | sync objs w/remote-node
    leo_recovery_node_queue         |   idling    | 0              | 1000           | 100            | recovery objs of node
    leo_async_deletion_queue        |   idling    | 0              | 1000           | 100            | async deletion of objs
    leo_rebalance_queue             |   running   | 2167           | 1400           | 10             | rebalance objs
    leo_sync_by_vnode_id_queue      |   idling    | 0              | 1000           | 100            | sync objs by vnode-id
    leo_per_object_queue            |   idling    | 0              | 1000           | 100            | recover inconsistent objs

.. _mq-suspend-command:

mq-suspend <storage-node> <mq-id>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: When turning on the watchdog mechanism, this command is ignored.

.. code-block:: bash

    $ ./leofs-adm mq-suspend storage_0@127.0.0.1 leo_delete_dir_queue
    OK

.. _mq-resume-command:

mq-resume <storage-node> <mq-id>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: When turning on the watchdog mechanism, this command is ignored.

.. code-block:: bash

    $ ./leofs-adm mq-resume storage_0@127.0.0.1 leo_delete_dir_queue
    OK

See Also
^^^^^^^^

* `LeoFS Storage configuration <../configuration/configuration_2.html>`_
* `LeoFS Watchdog configuration <../configuration/configuration_7.html>`_
