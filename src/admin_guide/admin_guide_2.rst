.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2015 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    General commands

General Commands
================

+-------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| **Shell**                                             | **Description**                                                                                   |
+=======================================================+===================================================================================================+
| leofs-adm :ref:`status <status-command>` [<node>]     | * Retrieve status of every node (default)                                                         |
|                                                       | * Retrieve status of the specified node                                                           |
+-------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`whereis <whereis-command>` <file-path>| Retrieve an assigned object by the file-path                                                      |
+-------------------------------------------------------+---------------------------------------------------------------------------------------------------+

.. _status-command:

.. index::
    pair: General commands; status-command

status
^^^^^^

Retrieve status of every node (default)

.. code-block:: bash

    -----------------------------------+----------
     Item                              | Value
    -----------------------------------+----------
     Basic/Consistency level
    -----------------------------------+----------
                        system version | 1.2.11
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
                     current ring-hash | 3923d007
                    previous ring-hash | 3923d007
    -----------------------------------+----------

     [State of Node(s)]
    -------+--------------------------+--------------+----------------+----------------+----------------------------
     type  |           node           |    state     |  current ring  |   prev ring    |          updated at
    -------+--------------------------+--------------+----------------+----------------+----------------------------
      S    | storage_0@127.0.0.1      | running      | 3923d007       | 3923d007       | 2015-07-02 15:28:06 +0900
      S    | storage_1@127.0.0.1      | running      | 3923d007       | 3923d007       | 2015-07-02 15:28:06 +0900
      S    | storage_2@127.0.0.1      | running      | 3923d007       | 3923d007       | 2015-07-02 15:28:06 +0900
      S    | storage_3@127.0.0.1      | running      | 3923d007       | 3923d007       | 2015-07-02 15:28:06 +0900
    -------+--------------------------+--------------+----------------+----------------+----------------------------


status <node>
^^^^^^^^^^^^^

Retrieve status of the specified node

.. code-block:: bash

    $ leofs-adm status storage_0@127.0.0.1
    --------------------------------------+--------------------------------------
                    Item                  |                 Value
    --------------------------------------+--------------------------------------
     Config-1: basic
    --------------------------------------+--------------------------------------
                                  version | 1.2.11
                         number of vnodes | 168
                        object containers | - path:[./avs], # of containers:8
                            log directory | ./log/erlang
    --------------------------------------+--------------------------------------
     Config-2: watchdog
    --------------------------------------+--------------------------------------
     [rex(rpc-proc)]                      |
                        check interval(s) | 5
                   threshold mem capacity | 33554432
    --------------------------------------+--------------------------------------
     [cpu]                                |
                         enabled/disabled | disabled
                        check interval(s) | 5
                   threshold cpu load avg | 5.0
                    threshold cpu util(%) | 100
    --------------------------------------+--------------------------------------
     [disk]                               |
                         enabled/disalbed | disabled
                        check interval(s) | 5
                    threshold disk use(%) | 100
                   threshold disk util(%) | 90
                        threshold rkb(kb) | 131072
                        threshold wkb(kb) | 131072
    --------------------------------------+--------------------------------------
     Config-3: message-queue
    --------------------------------------+--------------------------------------
                       number of procs/mq | 8
            number of batch-procs of msgs | max:1000, regular:400
       interval between batch-procs (ms)  | max:3000, regular:500
    --------------------------------------+--------------------------------------
     Config-4: autonomic operation
    --------------------------------------+--------------------------------------
     [auto-compaction]                    |
                         enabled/disabled | disabled
            warning active size ratio (%) | 70
          threshold active size ratio (%) | 60
                 number of parallel procs | 1
                            exec interval | 3600
    --------------------------------------+--------------------------------------
     Config-5: data-compaction
    --------------------------------------+--------------------------------------
      limit of number of compaction procs | 4
            number of batch-procs of objs | max:500, regular:100
       interval between batch-procs (ms)  | max:1000, regular:300
    --------------------------------------+--------------------------------------
     Status-1: RING hash
    --------------------------------------+--------------------------------------
                        current ring hash | 3923d007
                       previous ring hash | 3923d007
    --------------------------------------+--------------------------------------
     Status-2: Erlang VM
    --------------------------------------+--------------------------------------
                               vm version | 6.4
                          total mem usage | 41476672
                         system mem usage | 23647720
                          procs mem usage | 17850112
                            ets mem usage | 5844976
                                    procs | 364/1048576
                              kernel_poll | true
                         thread_pool_size | 32
    --------------------------------------+--------------------------------------
     Status-3: Number of messages in MQ
    --------------------------------------+--------------------------------------
                     replication messages | 0
                      vnode-sync messages | 0
                       rebalance messages | 0
    --------------------------------------+--------------------------------------

    $ leofs-adm status gateway_0@127.0.0.1
    -------------------------------+------------------
                 Item              |       Value
    -------------------------------+------------------
     Config-1: basic
    --------------------------------------------------
     [basic]
    -------------------------------+------------------
                           version | 1.2.11
                    using protocol | s3
                     log directory | ./log/erlang
    -------------------------------+------------------
     [http server related for rest/s3 api]
    -------------------------------+------------------
                    listening port | 8080
                listening ssl port | 8443
               number of acceptors | 128
    -------------------------------+------------------
     [cache-related]
    -------------------------------+------------------
           http cache [true/false] | false
           number of cache_workers | 16
                      cache expire | 300
             cache max content len | 1048576
                ram cache capacity | 0
            disk cache capacity    | 0
            disk cache threshold   | 1048576
            disk cache data dir    | ./cache/data
            disk cache journal dir | ./cache/journal
    -------------------------------+------------------
     [large object related]
    -------------------------------+------------------
          max number of chunk objs | 1000
               chunk object length | 5242880
                 max object length | 5242880000
         reading  chunk obj length | 5242880
         threshold of chunk length | 5767168
    -------------------------------+------------------
     Config-2: watchdog
    -------------------------------+------------------
     [rex(rpc-proc)]               |
                 watch interval(s) | 5
            threshold mem capacity | 33554432
    -------------------------------+------------------
     [cpu]                         |
                  eanbled/disabled | disabled
                 check interval(s) | 5
            threshold cpu load avg | 5.0
             threshold cpu util(%) | 100
    -------------------------------+------------------
     [io]                          |
                  enabled/disabled | disabled
                 check interval(s) | 1
            threshold input size/s | 134217728
           threshold output size/s | 134217728
    -------------------------------+------------------
     Status-1: RING hash
    -------------------------------+------------------
                 current ring hash | 3923d007
                previous ring hash | 3923d007
    -------------------------------+------------------
     Status-2: Erlang VM
    -------------------------------+------------------
                        vm version | 6.4
                   total mem usage | 69980072
                  system mem usage | 52410432
                   procs mem usage | 17588560
                     ets mem usage | 6891272
                             procs | 439/1048576
                       kernel poll | true
                  thread pool size | 32
    -------------------------------+------------------

\

.. _whereis-command:

.. index::
    pair: General commands; whereis-command

\

whereis <file-path>
^^^^^^^^^^^^^^^^^^^

Retrieve an assigned object by the file-path
Paths used by `whereis` are ruled by :ref:`this rule <s3-path-label>`

.. code-block:: bash

    $ leofs-adm whereis leo/fast/storage.key

    ------+---------------------+----------------------------------+-------+------------+----------------------------------+----------------+----------------+----------------+----------------------------
     del? |           node      |             ring address         | size  | checksum   |        redundancy method         |  has children  |  total chunks  |     clock      |             when
    ------+---------------------+----------------------------------+-------+------------+----------------------------------+----------------+----------------+----------------+----------------------------
          | storage_0@127.0.0.1 | d552cfef6fa3ccc0bd55de509475754b | 2358K | d7685fa58b | copy                             | false          |              0 | 527ec4caed51c  | 2015-12-28 11:45:35 +0900
          | storage_1@127.0.0.1 | d552cfef6fa3ccc0bd55de509475754b | 2358K | d7685fa58b | copy                             | false          |              0 | 527ec4caed51c  | 2015-12-28 11:45:35 +0900

\

If you want to retrieve an object whose file-path contains spaces,
Enclose the file-path with double quotation.

.. code-block:: bash

    $ leofs-adm whereis "leo/fast/storage with space.key"

    ------+---------------------+----------------------------------+-------+------------+----------------------------------+----------------+----------------+----------------+----------------------------
     del? |           node      |             ring address         | size  | checksum   |        redundancy method         |  has children  |  total chunks  |     clock      |             when
    ------+---------------------+----------------------------------+-------+------------+----------------------------------+----------------+----------------+----------------+----------------------------
          | storage_0@127.0.0.1 | d552cfef6fa3ccc0bd55de509475754b | 2358K | d7685fa58b | copy                             | false          |              0 | 527ec4caed51c  | 2015-12-28 11:45:35 +0900
          | storage_1@127.0.0.1 | d552cfef6fa3ccc0bd55de509475754b | 2358K | d7685fa58b | copy                             | false          |              0 | 527ec4caed51c  | 2015-12-28 11:45:35 +0900

\

If you want to retrieve a chunk object which is part of a large object,
Append `\\n` and the chunk number to the file-path.

.. code-block:: bash

    $ leofs-adm whereis "leo/fast/storage.key\n1"

    ------+---------------------+----------------------------------+-------+------------+----------------------------------+----------------+----------------+----------------+----------------------------
     del? |           node      |             ring address         | size  | checksum   |        redundancy method         |  has children  |  total chunks  |     clock      |             when
    ------+---------------------+----------------------------------+-------+------------+----------------------------------+----------------+----------------+----------------+----------------------------
          | storage_0@127.0.0.1 | d552cfef6fa3ccc0bd55de509475754b | 2358K | d7685fa58b | copy                             | false          |              0 | 527ec4caed51c  | 2015-12-28 11:45:35 +0900
          | storage_1@127.0.0.1 | d552cfef6fa3ccc0bd55de509475754b | 2358K | d7685fa58b | copy                             | false          |              0 | 527ec4caed51c  | 2015-12-28 11:45:35 +0900


\

You're able to check a redundancy method of an object with the ``redundancy method`` column.

.. code-block:: bash

    $ leofs-adm whereis test/Tech_Conf_2015_1.pptx
    ------+---------------------+----------------------------------+-------+------------+----------------------------------+----------------+----------------+----------------+----------------------------
     del? |           node      |             ring address         | size  | checksum   |        redundancy method         |  has children  |  total chunks  |     clock      |             when
    ------+---------------------+----------------------------------+-------+------------+----------------------------------+----------------+----------------+----------------+----------------------------
          | storage_0@127.0.0.1 | d552cfef6fa3ccc0bd55de509475754b | 2358K | d7685fa58b | erasure_code, {k:10, m:4, vandrs}| false          |             14 | 527ec4caed51c  | 2015-12-28 11:45:35 +0900
          | storage_1@127.0.0.1 | d552cfef6fa3ccc0bd55de509475754b | 2358K | d7685fa58b | erasure_code, {k:10, m:4, vandrs}| false          |             14 | 527ec4caed51c  | 2015-12-28 11:45:35 +0900

\
