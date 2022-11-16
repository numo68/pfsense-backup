Usage
=====

**pfsense-backup** is a simple tool to fetch configuration backups from
the pfSense firewall.

Arguments
----------------

``-h``, ``--help``
   show the help message and exit.

``-c FILE``, ``--config FILE``
   the configuration file (default: ``~/.config/pfsense-backup/config.yml``)

``-o FILE``, ``--output FILE``
   the output file. The file name can contain ``strftime`` directives. If the argument
   is specified, ``directory``, ``name`` and ``keep`` fields of the configuration
   are ignored.

Configuration file
==================

The ``pfsense-backup`` needs a configuration file
(default ``~/.config/pfsense-backup/config.yml``). As the file contains secrets,
take care to set reasonable permissions. The file is in
the `YAML <https://yaml.org/>`_ format.

Configuration file
------------------

.. code-block:: yaml

      pfsense:
         url: https://pfsense
         user: admin
         password: ...
         ssl_verify: true|false|/path/to/custom_cert.pem
      output:
         directory: .
         name: "pfsense-%Y%m%d-%H%M.xml"
         keep: 12

All fields except ``password`` are optional.

``host`` is a host name or an IP address.

``name`` specifies the name of the output file. ``strftime`` directives
are allowed.

``keep`` removes all but the most recent ``*.xml`` files from the ``directory``,
that in this case has to be specified and has to be an absolute path.

``directory`` has to already exist. As the backup is not encrypted
and contains secrets the permissions should be set accordingly.
