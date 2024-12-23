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


Observability
-------------

The backups done can be exported through prometheus text-file format and consumed
by the node exporter's textfile collector. If the ``metrics`` key exists, the file
is atomically populated after each backup run.

.. code-block:: yaml

    metrics:
        directory: "/var/local/lib/prom_metrics"
        suffix: "my-pfsense"

``directory`` is mandatory and specifies the path of the directory passed as the
``--collector.textfile.directory`` for the ``node_exporter``. It has to already exist.
``suffix`` is optional and will be appended to the file name to distinguish metrics
files generated by different configurations. For the above configuration
the full file name will be ``/var/local/lib/prom_metrics/pfsense-backup-my-pfsense.prom``.