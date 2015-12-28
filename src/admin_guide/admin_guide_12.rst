.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2015 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    Erasure Code

Administration of the Erasure Coding Support
============================================

Since
-------

LeoFS v1.4.0

.. index::
    pair: Erasure Code; How to configure a redundancy method of a bucket

How to configure a redundancy method of a bucket
------------------------------------------------

LeoFS supports two redundancy methods which are ``replication`` and ``erasure code``. The default value of a redundancy method of a bucket is ``replication``. If you want to change the redundancy method of a bucket, you can use ``leofs-adm set-redundancy-method`` as below:

::

    ## erasure-coding
    $ leofs-adm set-redundancy-method <bucket> <access-key-id> erasure-code \
                                      <number-of-data-chunks> <number-of-conding-chunks>
    ## replication
    $ leofs-adm set-redundancy-method <bucket> <access-key-id> copy

\


.. index::
    pair: Erasure Code; How to write leo_storage.conf

How to write *leo_storage.conf*
----------------------------------------

LeoFS delivers two configuration items for the erasure coding support. You're able to configure an erasure coding library of a Leo's stroage node with ``erasure_code.lib`` and specify minimum length of an object with ``erasure_code.min_object_size`` whether it adopts *erasure code* or *replication*. If an object satisfies those conditions, LeoFS adopts *erasure code*.

::

    ## Library of the erasure-code
    ##   - lib: [vandrs | cauchyrs | liberation | isars]
    erasure_code.lib = isars

    ## Minimum length of an object for the erasure-coding - default:262144(byte)
    erasure_code.min_object_size = 262144

\

.. index::
    pair: Etasure Code; Leo's Erasure Code Library

Leo's Erasure Code Library
--------------------------

|leo_erasure| is a Erlang binding for the open sourced Erasure Coding library **JErasure** and **Intel's ersure code lib - ISA-L**.


.. |leo_erasure| raw:: html

   <a href="https://github.com/leo-project/leo_erasure" target="_blank">leo_erasure</a>


See Also
--------

* :ref:`LeoFS' storage configuration <conf_storage_label>`
* :ref:`the set-redundancy-method command <set-redundancy-method>`
* :ref:`the whereis command <whereis-command>`
