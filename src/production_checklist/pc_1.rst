.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2016 Rakuten, Inc.
.. https://leo-project.net/
.. =========================================================

.. index::
   Production Checklist about LeoFS

===========================
Production Checklist: LeoFS
===========================

.. index::
   pair: Production Checklist about LeoFS; What version should we use?

What version should we use?
---------------------------

Use the latest stable one.
With the version <= 1.3.0, LeoFS had a serious issue that may cause data-lost,
so that use at least >= 1.3.1.
Or in case you need to keep running LeoFS with older one for some reason,
make sure that ``large_object.reading_chunked_obj_len`` <= ``large_object.chunked_obj_len`` in leo_gateway.conf.
This setting prevent LeoFS from suffering |issue531|.


See Also:
    * |issue531|
    * `leo_gateway.conf <../configuration/configuration_3.html>`_

----


.. |LeoFS| raw:: html

   <a href="https://leo-project.net/leofs/" target="_blank">LeoFS</a>

.. |issue531| raw:: html

   <a href="https://github.com/leo-project/leofs/issues/531" target="_blank">Issue#531 (The last part of a large object can be broken with reading_chunked_obj_len > chunked_obj_len in leo_gateway.conf)</a>
