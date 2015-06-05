.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2015 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

Introduction
============

**LeoFS** is a highly scalable, fault-tolerant unstructured object storage system for the Web.

LeoFS provides **High Cost Performance Ratio**. It allows you to build LeoFS clusters using commodity hardware on top of the Linux operating system. LeoFS will provide very good performance even on commodity hardware. LeoFS will require a smaller cluster than other storage to achieve the same performance. LeoFS is also very easy to setup and to operate.

LeoFS provides **High Reliability** thanks to its great design on top of the Erlang/OTP capabilities. Erlang/OTP is known for being used in production systems for years with a solid nine nines (99.9999999%) availability, and LeoFS is no exception. A LeoFS system will stay up regardless of software errors or hardware failures happening inside the cluster.

LeoFS provides **High Scalability**. Adding and removing nodes is simple and quick, allowing you to react swiftly when your needs change. A LeoFS cluster can be thought as elastic storage that you can stretch as much and as often as you need.

.. image:: ../../_static/images/leofs-architecture.001.jpg
   :width: 760px


LeoFS consists of 3 applications - |leo_storage_detail|, |leo_gateway_detail| and |leo_manager_detail| which depend on Erlang.

|leo_gateway_detail| handles http-request and http-response from any clients when using REST-API OR S3-API. Also, it is already built in the object-cache mechanism *(memory and disk cache)*.

|leo_storage_detail| handles GET, PUT and DELETE objects as well as metadata. Also, it has *replicator*, *recoverer* and *queueing mechanism* in order to keep running a storage node and realise eventual consistency.

|leo_manager_detail| always monitors *LeoFS Gateway* and *LeoFS Storage* nodes. The main monitoring status are *Node status* and *RING's checksum* in order to realise to keep high availability and keep data consistency.


.. toctree::
   :hidden:

   goal
   milestone

.. |leo_gateway_detail| raw:: html

   <a href="http://leo-project.net/leofs/docs/architecture/leofs-gateway-detail.html" target="_blank">LeoFS Gateway</a>

.. |leo_storage_detail| raw:: html

   <a href="http://leo-project.net/leofs/docs/architecture/leofs-storage-detail.html" target="_blank">LeoFS Storage</a>

.. |leo_manager_detail| raw:: html

   <a href="http://leo-project.net/leofs/docs/architecture/leofs-manager-detail.html" target="_blank">LeoFS Manager</a>
