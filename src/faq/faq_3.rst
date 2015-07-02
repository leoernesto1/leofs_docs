.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2015 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   FAQ LeoFS administration

===========================
FAQ: LeoFS Administration
===========================

.. index::
   pair: FAQ LeoFS administration; Features


Where can I get the packages for LeoFS?
---------------------------------------

You can get the packages by platform from |leofs-website|.
LeoFS package for FreeBSD has been published at |fresh-ports| and for SmartOS has been published at |smartos-package|, which is made by |project-fifo|.

\

How can I run my LeoFS cluster automatically?
---------------------------------------------

Currently after restarting a storage node,  you need to issue the resume command with leofs-adm script manually like this.

.. code-block:: bash

    leofs-adm resume storage_0@127.0.0.1


We're planing to provide LeoFS's autonomous operation like `auto-compaction`, `auto-rebalance` and others with LeoFS v1.2, v1.4 and v2.0.
Actually, `auto-compaction` which was already supported with v1.2.

\

How do multiple users login into LeoFS Manager's console at the same time?
--------------------------------------------------------------------------

Answer 1:
^^^^^^^^^^

Actually, there is no login status in LeoFS, but the number of listening tcp connections is limited by leo_manger.conf (default: console.acceptors.cui = 3).
So while using default settings, 3 connections can be connected to a manager in parallel.

Answer 2:
^^^^^^^^^^

Since LeoFS v1.1.0, LeoFS have the more powerful alternative option |leofs-adm| command.
This command have not only same functionalities with the existing telnet way but also do NOT keep an tcp connection established for long time(connect only when issueing a command).
So you do not need to worry about the number of tcp connections.

\

The result of the du command can be different with the actual disk-usage
-------------------------------------------------------------------------

In order to reduce system resource usage for calculating the result of the du command, LeoFS keep that information on memory and when stopping itself, LeoFS save those data into a local file.
Then when restarting, LeoFS load those on memory.

So if LeoFS is stopped unintentionally like killing by OOM killer, those data can become inconsistent with actual usage.

If you get into this situation, you can recover those data by issueing the compact-start command to the node. after finishing the compaction, the result of the du command will be consitent with actual usage.

See Also:
    * `du-command <../admin_guide/admin_guide_5.html#du>`_

\

When issueing the recover node command the LeoFS can get into high load
------------------------------------------------------------------------

Since the `recover-node command <../admin_guide/admin_guide_4.html#recover-node-command>`_ can lead to issue lots of disk I/O and consume network bandwidth, if you face to issue the recover-node to multiple nodes at once, LeoFS can get into high load and become unresponsive. So we recommend you execute the recover-node command to target nodes one by one.

If this solution could not work for you, you're able to control how much recover-node consume system resources by changing the MQ-related parameters in `leo_storage.conf <../configuration/configuration_2.html>`_

See Also:
    * `recover-node command <../admin_guide/admin_guide_4.html#recover-node-command>`_
    * `leo_storage.conf <../configuration/configuration_2.html>`_

\


What should I do when Too many processes errors happen?
-------------------------------------------------------

LeoFS usually try to keep the number of Erlang processes as minimum as possible, but there are some exceptions when doing something asynchronously.

* Replicating an object to the non-primary assigned nodes
* Retrying to replicate an object when the previous attempt failed

Given that LeoFS suffered from very high load AND there are some nodes downed for some reason, The number of Erlang processes gradually have increased and might have reached the sysmte limit.

We recommend users to set an appropriate value which depends on your workload to the ``+P option``. Also if the ``+P option`` does NOT work for you, there are some possibilities that some external system resources like disk, network equipments have broken, Please check out the dmesg/syslog on your sysmtem.

See Also:
    * |erlang-p|

\


Why does starting a leo_storage using bitcask as metadata take too much time?
-----------------------------------------------------------------------------

When starting a leo_storage with bitcask, since leo_storage always call the ``bitcask:merge`` operation, starting process may take too much time if leo_storage stored lots of objects. We recommend users to replace ``bitcask`` with ``leveldb`` by using |b2l|.


How do I set "a number of containers" at LeoFS Storage configuration?
---------------------------------------------------------------------

Objects/files are stored into LeoFS Storage containers which are log-structured files. So LeoFS has the ``data-compaction`` mechanism in order to remove unncessary objects/files from the object-containers of LeoFS Storage.

LeoFS's performance is affected by the data-compaction. And also, LeoFS Storage temporally creates a new file of a object-container corresponding to the compaction target container, which means during the data-compaction needs disk space for the new file of object-container(s).

If ``write/update/delete operation`` is a lot, we recommend that the number of containers is 32 OR 64 because it's possible to make effect of the data-compaction at a minimum as much as possible.

In conclusiton:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Read operation > Write operation:
    * # of containers = 8
* A lot of Write/Update/Delete:
    * # of containers = 32 OR 64 *(depends on the disk capacity and performance)*


leo_storage can not start due to "enif_send_failed on non smp vm"
---------------------------------------------------------------------

When starting leo_storage on a single core machine which crashes with an erl_nif error.

.. code-block:: bash

    ## Error log
    enif_send: env==NULL on non-SMP VM
    Aborted (core dumped)


In this case, you have faced with |issue_120|.
You need to set a Erlang's VM flag - ``-smp`` in your leo_storage configuration - *leo_storage.conf* as follows:

.. code-block:: bash

    ## leo_storage.conf
    erlang.smp = enable


See also:
    * |erl_nif|
    * |leo_storage configuration|
    * |leo_storage.conf|


Why is the speed of rebalance/recover command too slow?
-------------------------------------------------------

You must hit |issue_359|.
Since this issue has been fixed with LeoFS v1.2.9, we'd recommend you upgrading to 1.2.9 or higher one, and Set an appropriate value for your environment to `mq.num_of_mq_procs` in your leo_storage.conf.

See also:
    * |leo_storage configuration|
    * |leo_storage.conf|


How to run LeoFS on docker container?
-------------------------------------

|hack4docker| also should work for LeoFS.


Why does LeoFS's SNMP I/F give me wrong values(0) instead of correct values?
----------------------------------------------------------------------------

You must hit |issue_361|.
Since this issue has been fixed with LeoFS v1.2.9, we'd recommend you upgrading to 1.2.9 or higher one.

When adding a new storage node, that node doesn't appear with `leofs-adm status` Why?
-------------------------------------------------------------------------------------

If you changed a WRONG node name before stopping the daemon,
As a result, when a new daemon was starting, it failed to detect that the previous was still running and
stop command did not work too.
Since you can notice this kind of mistake in error.log with LeoFS v1.2.9, we'd recommend you upgrading to 1.2.9 or higher one.

See also:
    * |report_on_google_group|

.. |leofs-adm| raw:: html

   <a href="https://github.com/leo-project/leofs/blob/master/leofs-adm" target="_blank">leofs-adm</a>

.. |leofs-website| raw:: html

   <a href="http://leo-project.net/leofs/download.html" target="_blank">LeoFS download page</a>

.. |fresh-ports| raw:: html

   <a href="http://www.freshports.org/databases/leofs" target="_blank">Fresh ports/database</a>

.. |smartos-package| raw:: html

   <a href="http://release.project-fifo.net/pkg/rel/" target="_blank">LeoFS packages for SmartOS</a>

.. |project-fifo| raw:: html

   <a href="http://project-fifo.net" target="_blank">Project FiFo</a>

.. |erlang-p| raw:: html

   <a href="http://erlang.org/doc/man/erl.html#+P" target="_blank">Erlang - +P</a>

.. |b2l| raw:: html

   <a href="https://github.com/leo-project/leofs_utils/tree/develop/tools/b2l" target="_blank">Tool:Converting metadata from bitcask to leveldb</a>

.. |erl_nif| raw:: html

   <a href="http://www.erlang.org/doc/man/erl_nif.html#enif_send" target="_blank">Erlang - erl_nif</a>

.. |leo_storage configuration| raw:: html

   <a href="http://leo-project.net/leofs/docs/configuration/configuration_2.html" >LeoFS documentation - leo_storage configuration</a>

.. |leo_storage.conf| raw:: html

   <a href="https://github.com/leo-project/leo_storage/blob/develop/priv/leo_storage.conf" target="_blank">leo_storage - leo_storage.conf</a>

.. |issue_120| raw:: html

   <a href="https://github.com/basho/eleveldb/issues/120" target="_blank">eleveldb's issue#120</a>

.. |issue_359| raw:: html

   <a href="https://github.com/leo-project/leofs/issues/359" target="_blank">LeoFS's issue#359</a>

.. |hack4docker| raw:: html

   <a href="https://stackoverflow.com/questions/17252356/run-a-service-automatically-in-a-docker-container/17673148#17673148" target="_blank">This hack</a>

.. |issue_361| raw:: html

   <a href="https://github.com/leo-project/leofs/issues/361" target="_blank">LeoFS's issue#361</a>

.. |report_on_google_group| raw:: html

   <a href="https://groups.google.com/forum/#!topic/leoproject_leofs/GkO_D3f-SIo" target="_blank">Troubleshooting Adding a Storage Node</a>
