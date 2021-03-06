Running Openchain
=================

Deploying Openchain server can be done :ref:`through Docker <docker-deployment>`.

This document explains how to deploy Openchain directly on a machine without using docker.

Prerequisites
-------------

Install the `.NET Command Line Interface <https://www.microsoft.com/net/core>`_ . This is cross-platform and runs on Windows, Linux and OS X.

Download the project files
--------------------------

Download the ``project.json``, ``Program.cs`` and ``config.json`` files from GitHub, then restore the NuGet dependencies. On Linux:

.. code-block:: bash

    $ wget https://raw.githubusercontent.com/openchain/openchain/v0.6.2/src/Openchain/project.json
    $ wget https://raw.githubusercontent.com/openchain/openchain/v0.6.2/src/Openchain/Program.cs
    $ wget https://raw.githubusercontent.com/openchain/openchain/v0.6.2/src/Openchain/data/config.json -P data
    $ dotnet restore

.. note:: On Windows, simply download the files manually using your browser, then run ``dotnet restore``.

Run Openchain Server
--------------------

Run openchain server using the following command:

.. code-block:: bash

    $ dotnet run

Configuration
-------------

The dependencies section of the ``project.json`` file references the external providers pulled from NuGet:

.. code-block:: json
   :emphasize-lines: 9-12

    "dependencies": {
      "Microsoft.NETCore.App": {
        "version": "1.0.0",
        "type": "platform"
      },
      "Microsoft.AspNetCore.Server.IISIntegration": "1.0.0",
      "Openchain.Server": "0.6.2",
    
      "Openchain.Anchoring.Blockchain": "0.6.2",
      "Openchain.Sqlite": "0.6.2",
      "Openchain.SqlServer": "0.6.2",
      "Openchain.Validation.PermissionBased": "0.6.2"
    },

By defaut, this imports the Sqlite storage engine (``Openchain.Sqlite``), the SQL Server storage engine (``Openchain.SqlServer``), the permission-based validation module (``Openchain.Validation.PermissionBased``), and the Blockchain anchoring module (``Openchain.Anchoring.Blockchain``). Update this list with the modules (and versions) you want to import.

You can then edit the ``data/config.json`` file to reference the :ref:`providers you want to use <configuration>`.

.. tip:: For example, if you want to use the ``SQLite`` provider as a storage engine, you will need to make sure the ``Openchain.Sqlite`` module is listed in the dependencies.

Make sure you run ``dotnet restore`` again after modifying project.json.

.. note:: The Openchain.Server dependency is the only one that is always required. The version of the ``Openchain.Server`` package is the version of Openchain you will be running.

Updating the target platform
----------------------------

The frameworks section of the project.json file lists the available target frameworks:

.. code-block:: json

    "frameworks": {
      "netcoreapp1.0": {},
      "net451": {}
    }

By default .NET Core (cross-platform) and the .NET Framework (Windows only) are both targeted. Some providers run only on a subset of frameworks. In that case, remove the unsupported frameworks from the list to ensure the project runs.
