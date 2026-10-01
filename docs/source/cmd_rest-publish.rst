..  _ksconf_cmd_rest-publish:

ksconf rest-publish
===================

..  note::

    This command effectively replaces :ref:`ksconf_cmd_rest-export` for nearly all use cases.
    The only thing that ``rest-publish`` can't do that ``rest-export`` can, is handle a disconnected scenario.
    But for **ALL** other use cases, the ``rest-publish`` (this command) command is far superior.

..  note:: This commands requires the Splunk Python SDK, which is automatically bundled with the *Splunk app for KSCONF*.


..  argparse::
    :module: ksconf.cli
    :func: build_cli_parser
    :path: rest-publish
    :nodefault:

    -m, --meta:
        Note that ``mtime`` is ignored as that attribute is updated automatically every time a change occurs.
        There is no known way to work around this without file system access.

--------



Examples
---------

A simple example:

.. code-block:: sh

    ksconf rest-publish etc/app/Splunk_TA_aws/local/props.conf \
        --user admin --password secret --app Splunk_TA_aws --owner nobody --sharing global


Publishing to ``system/local``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To write settings to ``$SPLUNK_HOME/etc/system/local``, use the ``system`` app along with the default owner of ``nobody``:

.. code-block:: sh

    ksconf rest-publish props.conf --user admin --password secret --app system --owner nobody

The ``--owner nobody`` part is the default, so it can be omitted.

Existing stanzas
~~~~~~~~~~~~~~~~

By default, if a matching stanza already exists, then updates are applied within the namespace where that stanza was found.
This is often exactly what you want.
However, when a stanza is shared globally by another app, then ``--app`` and ``--owner`` do not influence where the changes are written, and the update happens in the original app.

Use ``--force-namespace`` to always write to the namespace given by ``--app`` and ``--owner``.
For example, to write a copy of a stanza that currently lives in another app to ``system/local``:

.. code-block:: sh

    ksconf rest-publish props.conf --user admin --password secret \
        --app system --owner nobody --force-namespace

The original stanza is not removed or modified.

..  warning::

    Splunk does not write a setting if its value is identical to the value already in effect for that stanza.
    So with ``--force-namespace``, settings that differ from the existing stanza are written to the requested namespace, but settings that already match are skipped.
    This means that copying an unchanged stanza to a new location can create an empty stanza.

    The REST API also typically shows only one of the matching stanzas, so a repeat run with ``--force-namespace`` can report an update even though the settings are already in place.

Replaying metadata
~~~~~~~~~~~~~~~~~~

This command also supports replaying metdata like ACLs:

.. code-block:: sh

    ksconf rest-publish etc/app/Splunk_TA_aws/local/props.conf \
        --meta etc/app/Splunk_TA_aws/metdata/local.meta \
        --user admin --password secret --app Splunk_TA_aws
