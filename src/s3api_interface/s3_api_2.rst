.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2015 Rakuten, Inc.
.. https://leo-project.net/
.. =========================================================

Amazon S3 Interface
===================

.. index::
   pair: Official Documents; Amazon S3 Interface

Amazon S3 Official Documentation
""""""""""""""""""""""""""""""""

* |s3_docs_root|

.. _s3-path-label:

How to determine the name of Bucket
"""""""""""""""""""""""""""""""""""

.. index::
   pair: How to determine the name of Bucket; Installation

*  |s3_docs_vhost|
*   Values used for determining the name:
    * S3 uses the **HTTP Host header** or a **Path segment in the HTTP request line**.
    * How S3 determines what to use depends on the **Domain name**.
*   Patterns

**1. https://s3.amazonaws.com/bucket/path_to_file**

  In this case, the name of the bucket is the first segment of the path.

  `bucket`

  +--------------+--------------------------------------------------------+
  | Name         | Value                                                  |
  +==============+========================================================+
  | Request line | GET /bucket/path_to_file HTTP/1.1                      |
  +--------------+--------------------------------------------------------+
  | Host header  | Host: s3.amazonaws.com                                 |
  +--------------+--------------------------------------------------------+

  The argument of LeoFS' :ref:`whereis <whereis-command>` | :ref:`purge-cache <purge>` commands should be `bucket/path_to_file`.

**2. https://www.example.com.s3.amazonaws.com/path_to_file**

  In this case, the name of the bucket is the part of the domain name before `.s3.amazonaws.com`.

  `www.example.com`

  +--------------+--------------------------------------------------------+
  | Name         | Value                                                  |
  +==============+========================================================+
  | Request line | GET /path_to_file HTTP/1.1                             |
  +--------------+--------------------------------------------------------+
  | Host header  | Host: www.example.com.s3.amazonaws.com                 |
  +--------------+--------------------------------------------------------+

  The argument of LeoFS' :ref:`whereis <whereis-command>` | :ref:`purge <purge>` commands should be `www.example.com/path_to_file`.

**3. https://www.example.com/path_to_file**

  In this case, the name of bucket is equal to the FQDN.

    `www.example.com`

  +--------------+--------------------------------------------------------+
  | Name         | Value                                                  |
  +==============+========================================================+
  | Request line | GET /path_to_file HTTP/1.1                             |
  +--------------+--------------------------------------------------------+
  | Host header  | Host: www.example.com                                  |
  +--------------+--------------------------------------------------------+

  The argument of LeoFS' :ref:`whereis <whereis-command>` | :ref:`purge <purge>` commands should be `www.example.com/path_to_file`.


**4. Rules for Bucket Naming (v0.16.1-)**

.. note::  We recommend as a best practice that you always use DNS-compliant bucket names regardless of the region in which you create the bucket.

* Bucket names can be as long as between 3 and 255 characters.
* Bucket names can contain lowercase letters, numbers, periods (.), dashes (-) and underscores (_).
* Bucket names must not be formatted as an IP address (e.g., 192.168.5.4).
* Bucket name cannot start and end with periods (.), dashes (-) and underscores (_).

.. |s3_docs_vhost| raw:: html

   <a href="https://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html" target="_blank">Virtual Hosting of Buckets</a>

.. |s3_docs_root| raw:: html

   <a href="https://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html" target="_blank">The official documentation</a>
