.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2015 Rakuten, Inc.
.. https://leo-project.net/
.. =========================================================

.. index::
   FAQ LeoFS limits

=======================
FAQ: LeoFS Limits
=======================

.. index::
   pair: FAQ LeoFS limits; Features

Features
--------

* LeoFS have covered almost major |AmazonS3API| but not all.
* If you use |MultiPartUploadAPI|, the size of a part of an object must be less than the size of a chunked object in LeoFS.
* When using the Multi DC feature, we have supported up to 2 clusters with LeoFS v1.2.8 or lowers, but we will support over 3 clusters with LeoFS v1.4.

See Also:
    * |s3_api_interface|
    * |configuration_3|
    * |ISSUE_177|
    * |ISSUE_190|
    * |ISSUE_338|


----

.. index::
   pair: FAQ LeoFS limits; Operations

Operations
----------

* When you upgrade your LeoFS, you can NOT change the metadata storage as |KVS| - ``bitcask`` or ``leveldb`` can be used in LeoFS - used by LeoFS Storage. We recommend users to replace ``bitcask`` with ``leveldb`` by using |leofs_utils/tools/b2l|.

See Also:
    * |configuration_2|
    * |admin_guide_10|
    * |leofs_utils/tools/b2l|

----

.. index::
   pair: FAQ LeoFS limits; NFS support

NFS Support
-----------

* NFS implemantation with LeoFS v1.1 is a subset of |NFSv3|. Lock manager protocol, ``Authentication``, and ``Owner/Permission`` management are NOT covered.
* The `ls` command may take too much time when the target directory have lots of child. We're planning to provide better performance with LeoFS v.1.4.
* If you use LeoFS with NFS, you should set the size of a chunked object in LeoFS to 1MB (1048576Bytes) - ``large_object.chunked_obj_len = 1048576`` *(leo_gateway.conf)*, otherwise the efficiency of disk utilization can be decreased.
* What are the requirements to run LeoFS with NFS?

  * LeoFS Gateway(NFS Server)

    We've supporeted the targets Debian 6, Ubuntu-Server 14.04 LTS or Higher and CentOS 6.5/7.0 as LeoFS does, but should work on most linux platforms. In addition, We've confirmed LeoFS with NFS works properly on the latest FreeBSD and SmartOS by using |NFS_TEST_TOOL|.

  * NFS Client

    Since we still have not implemented NFS Lock Manager, you can only use NFS Clients which support disabling lock option(the linux client might be the only option for now). After we will finish implementing NFS Lock Manager, You can use NFS Client on other xnix platforms as well.

See Also:
    * |configuration_3|
    * |ISSUE_198|
    * |NFS_LOCK_MANAGER_PROTO|

----


.. |AmazonS3API| raw:: html

   <a href="https://docs.aws.amazon.com/AmazonS3/latest/API/APIRest.html" target="_blank">Amazon S3 REST API</a>

.. |MultiPartUploadAPI| raw:: html

   <a href="https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html" target="_blank">Amazon S3 multipart upload API</a>

.. |KVS| raw:: html

   <a href="https://en.wikipedia.org/wiki/Key/value_store#Key.E2.80.93Value_or_KV_stores" target="_blank">KVS</a>

.. |NFSv3| raw:: html

   <a href="https://www.ietf.org/rfc/rfc1813.txt" target="_blank">NFS v3</a>

.. |ISSUE_198| raw:: html

   <a href="https://github.com/leo-project/leofs/issues/198" target="_blank">NFS R/W transfer block size is limited up to 1MB</a>

.. |ISSUE_177| raw:: html

   <a href="https://github.com/leo-project/leofs/issues/177" target="_blank">Respond an incorrect MD5 of an large object</a>

.. |ISSUE_190| raw:: html

   <a href="https://github.com/leo-project/leofs/issues/190" target="_blank">Multipart uploads of large files produces partially corrupted data when upload chunk size</a>

.. |leofs_utils/tools/b2l| raw:: html

   <a href="https://github.com/leo-project/leofs_utils/tree/develop/tools/b2l" target="_blank">leofs_utils/tools/b2l</a>

.. |NFS_TEST_TOOL| raw:: html

   <a href="https://github.com/leo-project/leo_gateway/blob/develop/test/leo_nfs_integration_tests.sh" target="_blank">NFS Integration Test Tool</a>

.. |NFS_LOCK_MANAGER_PROTO| raw:: html

   <a href="https://tools.ietf.org/html/rfc1813#page-114" target="_blank">NFS Lock Manager Protocol</a>

.. |s3_api_interface| raw:: html

   <a href="https://leo-project.net/leofs/docs/s3api_interface/s3_api.html" target="_blank">Amazon S3 API and Interface</a>

.. |configuration_2| raw:: html

   <a href="https://leo-project.net/leofs/docs/configuration/configuration_2.html" target="_blank">Configuration of LeoFS Storage</a>

.. |configuration_3| raw:: html

   <a href="https://leo-project.net/leofs/docs/configuration/configuration_3.html" target="_blank">Configuration of Gateway nodes</a>

.. |admin_guide_10| raw:: html

   <a href="https://leo-project.net/leofs/docs/admin_guide/admin_guide_10.html" target="_blank">Upgrade your old version LeoFS to v1.2.7</a>

.. |ISSUE_338| raw:: html

   <a href="https://github.com/leo-project/leofs/issues/338" target="_blank">Support over 3 clusters</a>
