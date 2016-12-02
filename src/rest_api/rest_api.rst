.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2015 Rakuten, Inc.
.. https://leo-project.net/
.. =========================================================

REST API
========

.. index::
   pair: REST API; Interface

Configuration
-------------

* Update ``LeoFS Gateway HTTP API`` in :ref:`your LeoFS Gateway configuration <conf_gateway_label>`

::

    protocol = rest

\


Interface
---------

Description of LeoFS' behavior for each HTTP verb
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

\

+----------------+--------------------------------------------------------+
| HTTP Verb      | LeoFS' Behavior                                        |
+================+========================================================+
| PUT/POST       | Insert an object into the storage cluster              |
+----------------+--------------------------------------------------------+
| GET            | Retrieve an object from the storage cluster            |
+----------------+--------------------------------------------------------+
| DELETE         | Remove an object from the storage cluster              |
+----------------+--------------------------------------------------------+

URL format
^^^^^^^^^^

* URL format: https://${HOST}:8080/**<file-path>**
    * LeoFS will only use the ``<file-path>`` part of the URL to identify objects
    * You can check that an object exists in the cluster by using:

::

    $ leofs-adm whereis <file-path>


.. index::
   pair: REST API; Examples

Examples
--------

POST/PUT

::

    $ curl -X POST -H "Content-Type:image/png" \
              --data-binary @test.jpg https://hostname:8080/_test/_image/file.png

    $ curl -X PUT -H "Content-Type:image/png" \
              --data-binary @test.jpg https://hostname:8080/_test/_image/file.png

GET

::

    $ curl -X GET https://hostname:8080/_test/_image/file.png
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 6019k    0 6019k    0     0   210M      0 --:--:-- --:--:-- --:--:--  217M

DELETE

::

    $ curl -X DELETE https://hostname:8080/_test/_image/file.png

