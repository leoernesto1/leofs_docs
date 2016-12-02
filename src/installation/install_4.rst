.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2015 Rakuten, Inc.
.. https://leo-project.net/
.. =========================================================

.. index::
   pair: Firewall Rules; Installation

Firewall Rules
--------------

In order for LeoFS to work correctly, it is necessary to set and check the firewall rules in your environment as follows:

+----------------------+-----------+-----------------+--------------------------+
| Subsystem            | Direction | Ports           | Notes                    |
+======================+===========+=================+==========================+
| LeoFS Manager-Master | Incoming  | 10010/*         | Manager console          |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Master | Incoming  | 4369/*          | Erlang Port Mapper       |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Master | Incoming  | 4020/*          | SNMP Listen Port         |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Master | Outgoing  | \*/4369         | Erlang Port Mapper       |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Slave  | Incoming  | 10011/*         | Manager console          |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Slave  | Incoming  | 4369/*          | Erlang Port Mapper       |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Slave  | Incoming  | 4021/*          | SNMP Listen Port         |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Slave  | Outgoing  | \*/4369         | Erlang Port Mapper       |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Storage        | Incoming  | 4369/*          | Erlang Port Mapper       |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Storage        | Incoming  | 4010/*          | SNMP Listen Port         |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Storage        | Outgoing  | \*/4369         | Erlang Port Mapper       |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Gateway        | Incoming  | 8080/*          | HTTP listen port         |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Gateway        | Incoming  | 8443/*          | HTTPS listen port        |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Gateway        | Incoming  | 4369/*          | Erlang Port Mapper       |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Gateway        | Incoming  | 4000/*          | SNMP Listen Port         |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Gateway        | Outgoing  | \*/4369         | Erlang Port Mapper       |
+----------------------+-----------+-----------------+--------------------------+
| ALL                  | Both      | [#]_            | Erlang RPC to others     |
+----------------------+-----------+-----------------+--------------------------+

.. [#] Port range can be specified by setting the kernel variables 'inet_dist_listen_min' AND 'inet_dist_listen_max'

Example

.. code-block:: erlang

    %%% This forces Erlang to use only ports 9100--9105 for distributed Erlang traffic.
    application:set_env(kernel, inet_dist_listen_min, 9100).
    application:set_env(kernel, inet_dist_listen_max, 9105).

.. raw:: html

    <div style="margin-bottom:80px;">&nbsp;</div>
