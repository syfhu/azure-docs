---
title: How To Dump and Restore in Azure Database for PostgreSQL
description: Describes how to extract a PostgreSQL database into a dump file and restore from a file created by pg_dump in Azure Database for PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/01/2018
---
# Migrate your PostgreSQL database using dump and restore
You can use [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) to extract a PostgreSQL database into a dump file and [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) to restore the PostgreSQL database from an archive file created by pg_dump.

## Prerequisites
To step through this how-to guide, you need:
- An [Azure Database for PostgreSQL server](quickstart-create-server-database-portal.md) with firewall rules to allow access and database under it.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) and [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) command-line utilities installed

Follow these steps to dump and restore your PostgreSQL database:

## Create a dump file using pg_dump that contains the data to be loaded
To back up an existing PostgreSQL database on-premises or in a VM, run the following command:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
For example, if you have a local server and a database called **testdb** in it
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

> [!IMPORTANT]
> Copy the backup files to an Azure blob/store and perform the restore from there, which should be a lot faster than performing the restore across the Internet.
> 

## Restore the data into the target Azure Database for PostrgeSQL using pg_restore
Once you have created the target database, you can use the pg_restore command and the -d, --dbname parameter to restore the data into the target database from the dump file.
```bash
pg_restore -v --no-owner –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Including the --no-owner parameter causes all objects created during the restore to be owned by the user specified with --username. For more information, see the official PostgreSQL documentation on [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).

In this example, restore the data from the dump file **testdb.dump** into the database **mypgsqldb** on target server **mydemoserver.postgres.database.azure.com**. 
```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## Next steps
- To migrate a PostgreSQL database using export and import, see [Migrate your PostgreSQL database using export and import](howto-migrate-using-export-and-import.md).
- For more information about migrating databases to Azure Database for PostgreSQL, see the [Database Migration Guide](http://aka.ms/datamigration).
