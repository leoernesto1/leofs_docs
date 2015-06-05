.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2015 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   pair: Architecture; LeoFS Manager

LeoFS Manager
=============

*LeoFS Manager* generates and manages a routing table, which is called **RING** and based on `consistent hashing <http://en.wikipedia.org/wiki/Consistent_hashing>`_.

*LeoFS Manager* always monitors every `LeoFS Storage <leofs-storage-detail.html>`_ and `LeoFS Gateway <leofs-gateway-detail.html>`_ of status and RING in order to keep running LeoFS and consistency of a RING. And also, it distributes RING to `LeoFS Storage <leofs-storage-detail.html>`_ and `LeoFS Gateway <leofs-gateway-detail.html>`_.

.. image:: ../../_static/images/leofs-architecture.007.jpg
   :width: 760px

In addition, *LeoFS Manager* provides |admin_commands| to be able to easily operate LeoFS.
*LeoFS administration commands* already cover entire LeoFS functions.

.. |admin_commands| raw:: html

   <a href="http://leo-project.net/leofs/docs/admin_guide/admin_commands.html" target="_blank">LeoFS administration commands</a>
