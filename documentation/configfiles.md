---
layout: documentation
menu: configfiles
subtitle: Config Files
---
# Config Files

Flyway optionally supports loading configuration via config files.

## Structure

Config files have the following structure:

```properties
# Settings are simple key-value pairs
flyway.key=value
# Single line comment start with a hash

# These are some example settings
flyway.url=jdbc:mydb://mydatabaseurl
flyway.schemas=schema1,schema2
flyway.placeholders.keyABC=valueXYZ
```

## Reference

These are the settings supported via config files:

```properties
# JDBC url to use to connect to the database
# Examples
# --------
# Most drivers are included out of the box.
# * = JDBC driver must be downloaded and installed in /drivers manually
# ** = TNS_ADMIN environment variable must point to the directory of where tnsnames.ora resides
# Aurora MySQL      : jdbc:mysql://<instance>.<region>.rds.amazonaws.com:<port>/<database>?<key1>=<value1>&<key2>=<value2>...
# Aurora PostgreSQL : jdbc:postgresql://<instance>.<region>.rds.amazonaws.com:<port>/<database>?<key1>=<value1>&<key2>=<value2>...
# CockroachDB       : jdbc:postgresql://<host>:<port>/<database>?<key1>=<value1>&<key2>=<value2>...
# DB2*              : jdbc:db2://<host>:<port>/<database>
# Derby             : jdbc:derby:<subsubprotocol>:<database><;attribute=value>
# Firebird          : jdbc:firebirdsql://<host>[:<port>]/<database>?<key1>=<value1>&<key2>=<value2>...
# H2                : jdbc:h2:<file>
# HSQLDB            : jdbc:hsqldb:file:<file>
# Informix*         : jdbc:informix-sqli://<host>:<port>/<database>:informixserver=dev
# MariaDB           : jdbc:mariadb://<host>:<port>/<database>?<key1>=<value1>&<key2>=<value2>...
# MySQL             : jdbc:mysql://<host>:<port>/<database>?<key1>=<value1>&<key2>=<value2>...
# Oracle*           : jdbc:oracle:thin:@//<host>:<port>/<service>
# Oracle* (TNS)**   : jdbc:oracle:thin:@<tns_entry>
# PostgreSQL        : jdbc:postgresql://<host>:<port>/<database>?<key1>=<value1>&<key2>=<value2>...
# SAP HANA*         : jdbc:sap://<host>:<port>/?databaseName=<database>
# SQL Server        : jdbc:sqlserver:////<host>:<port>;databaseName=<database>
# SQLite            : jdbc:sqlite:<database>
# Sybase ASE        : jdbc:jtds:sybase://<host>:<port>/<database>
# Redshift*         : jdbc:redshift://<host>:<port>/<database>
flyway.url=

# Fully qualified classname of the JDBC driver (autodetected by default based on flyway.url)
# flyway.driver=

# User to use to connect to the database. Flyway will prompt you to enter it if not specified.
# flyway.user=

# Password to use to connect to the database. Flyway will prompt you to enter it if not specified.
# flyway.password=

# The maximum number of retries when attempting to connect to the database. After each failed attempt,
# Flyway will wait 1 second before attempting to connect again, up to the maximum number of times specified
# by connectRetries. (default: 0)
# flyway.connectRetries=

# The SQL statements to run to initialize a new database connection immediately after opening it. (default: none)
# flyway.initSql=

# Comma-separated list of schemas managed by Flyway. These schema names are case-sensitive.
# Consequences:
# - Flyway will automatically attempt to create all these schemas, unless the first one already exists.
# - The first schema in the list will be automatically set as the default one during the migration.
# - The first schema in the list will also be the one containing the schema history table.
# - The schemas will be cleaned in the order of this list.
# - If Flyway created them, the schemas themselves will as be dropped when cleaning.
# (default: The default schema for the database connection)
# flyway.schemas=

# Name of Flyway's schema history table (default: flyway_schema_history)
# By default (single-schema mode) the schema history table is placed in the default schema for the connection
# provided by the datasource.
# When the flyway.schemas property is set (multi-schema mode), the schema history table is placed in the first
# schema of the list.
# flyway.table=

# The tablespace where to create the schema history table that will be used by Flyway.
# This setting is only relevant for databases that do support the notion of tablespaces. It's value is simply
# ignored for all others. (default: The default tablespace for the database connection)
# flyway.tablespace=

# Comma-separated list of locations to scan recursively for migrations. (default: filesystem:<<INSTALL-DIR>>/sql)
# The location type is determined by its prefix.
# Unprefixed locations or locations starting with classpath: point to a package on the classpath and may contain
# both SQL and Java-based migrations.
# Locations starting with filesystem: point to a directory on the filesystem, may only
# contain SQL migrations and are only scanned recursively down non-hidden directories.
# flyway.locations=

# Comma-separated list of fully qualified class names of custom MigrationResolver to use for resolving migrations.
# flyway.resolvers=

# If set to true, default built-in resolvers (jdbc, spring-jdbc and sql) are skipped and only custom resolvers as
# defined by 'flyway.resolvers' are used. (default: false)
# flyway.skipDefaultResolvers=

# Comma-separated list of directories containing JDBC drivers and Java-based migrations.
# (default: <INSTALL-DIR>/jars)
# flyway.jarDirs=

# File name prefix for versioned SQL migrations (default: V)
# Versioned SQL migrations have the following file name structure: prefixVERSIONseparatorDESCRIPTIONsuffix ,
# which using the defaults translates to V1_1__My_description.sql
# flyway.sqlMigrationPrefix=

# The file name prefix for undo SQL migrations. (default: U)
# Undo SQL migrations are responsible for undoing the effects of the versioned migration with the same version.
# They have the following file name structure: prefixVERSIONseparatorDESCRIPTIONsuffix ,
# which using the defaults translates to U1.1__My_description.sql
# Flyway Pro and Flyway Enterprise only
# flyway.undoSqlMigrationPrefix=

# File name prefix for repeatable SQL migrations (default: R)
# Repeatable SQL migrations have the following file name structure: prefixSeparatorDESCRIPTIONsuffix ,
# which using the defaults translates to R__My_description.sql
# flyway.repeatableSqlMigrationPrefix=

# File name separator for Sql migrations (default: __)
# Sql migrations have the following file name structure: prefixVERSIONseparatorDESCRIPTIONsuffix ,
# which using the defaults translates to V1_1__My_description.sql
# flyway.sqlMigrationSeparator=

# Comma-separated list of file name suffixes for SQL migrations. (default: .sql)
# SQL migrations have the following file name structure: prefixVERSIONseparatorDESCRIPTIONsuffix ,
# which using the defaults translates to V1_1__My_description.sql
# Multiple suffixes (like .sql,.pkg,.pkb) can be specified for easier compatibility with other tools such as
# editors with specific file associations.
# flyway.sqlMigrationSuffixes=

# Whether to stream SQL migrations when executing them. (default: false)
# Streaming doesn't load the entire migration in memory at once. Instead each statement is loaded individually.
# This is particularly useful for very large SQL migrations composed of multiple MB or even GB of reference data,
# as this dramatically reduces Flyway's memory consumption.
# Flyway Pro and Flyway Enterprise only
# flyway.stream=

# Whether to batch SQL statements when executing them. (default: false)
# Batching can save up to 99 percent of network roundtrips by sending up to 100 statements at once over the
# network to the database, instead of sending each statement individually. This is particularly useful for very
# large SQL migrations composed of multiple MB or even GB of reference data, as this can dramatically reduce
# the network overhead. This is supported for INSERT, UPDATE, DELETE, MERGE and UPSERT statements.
# All other statements are automatically executed without batching.
# Flyway Pro and Flyway Enterprise only
# flyway.batch=

# Encoding of SQL migrations (default: UTF-8)
# flyway.encoding=

# Whether placeholders should be replaced. (default: true)
# flyway.placeholderReplacement=

# Placeholders to replace in Sql migrations
# flyway.placeholders.user=
# flyway.placeholders.my_other_placeholder=

# Prefix of every placeholder (default: ${ )
# flyway.placeholderPrefix=

# Suffix of every placeholder (default: } )
# flyway.placeholderSuffix=

# Target version up to which Flyway should consider migrations.
# Defaults to 'latest'
# Special values:
# - 'current': designates the current version of the schema
# - 'latest': the latest version of the schema, as defined by the migration with the highest version
# flyway.target=

# Whether to automatically call validate or not when running migrate. (default: true)
# flyway.validateOnMigrate=

# Whether to automatically call clean or not when a validation error occurs. (default: false)
# This is exclusively intended as a convenience for development. even though we
# strongly recommend not to change migration scripts once they have been checked into SCM and run, this provides a
# way of dealing with this case in a smooth manner. The database will be wiped clean automatically, ensuring that
# the next migration will bring you back to the state checked into SCM.
# Warning ! Do not enable in production !
# flyway.cleanOnValidationError=

# Whether to disabled clean. (default: false)
# This is especially useful for production environments where running clean can be quite a career limiting move.
# flyway.cleanDisabled=

# The version to tag an existing schema with when executing baseline. (default: 1)
# flyway.baselineVersion=

# The description to tag an existing schema with when executing baseline. (default: << Flyway Baseline >>)
# flyway.baselineDescription=

# Whether to automatically call baseline when migrate is executed against a non-empty schema with no schema history
# table. This schema will then be initialized with the baselineVersion before executing the migrations.
# Only migrations above baselineVersion will then be applied.
# This is useful for initial Flyway production deployments on projects with an existing DB.
# Be careful when enabling this as it removes the safety net that ensures
# Flyway does not migrate the wrong database in case of a configuration mistake! (default: false)
# flyway.baselineOnMigrate=

# Allows migrations to be run "out of order" (default: false).
# If you already have versions 1 and 3 applied, and now a version 2 is found,
# it will be applied too instead of being ignored.
# flyway.outOfOrder=

# Whether Flyway should output a table with the results of queries when executing migrations (default: true).
# Flyway Pro and Flyway Enterprise only
# flyway.outputQueryResults=

# This allows you to tie in custom code and logic to the Flyway lifecycle notifications (default: empty).
# Set this to a comma-separated list of fully qualified class names of org.flywaydb.core.api.callback.Callback
# implementations.
# flyway.callbacks=

# If set to true, default built-in callbacks (sql) are skipped and only custom callback as
# defined by 'flyway.callbacks' are used. (default: false)
# flyway.skipDefaultCallbacks=

# Ignore missing migrations when reading the schema history table. These are migrations that were performed by an
# older deployment of the application that are no longer available in this version. For example: we have migrations
# available on the classpath with versions 1.0 and 3.0. The schema history table indicates that a migration with
# version 2.0 (unknown to us) has also been applied. Instead of bombing out (fail fast) with an exception, a
# warning is logged and Flyway continues normally. This is useful for situations where one must be able to deploy
# a newer version of the application even though it doesn't contain migrations included with an older one anymore.
# Note that if the most recently applied migration is removed, Flyway has no way to know it is missing and will
# mark it as future instead.
# true to continue normally and log a warning, false to fail fast with an exception. (default: false)
# flyway.ignoreMissingMigrations=

# Ignore ignored migrations when reading the schema history table. These are migrations that were added in between
# already migrated migrations in this version. For example: we have migrations available on the classpath with
# versions from 1.0 to 3.0. The schema history table indicates that version 1 was finished on 1.0.15, and the next
# one was 2.0.0. But with the next release a new migration was added to version 1: 1.0.16. Such scenario is ignored
# by migrate command, but by default is rejected by validate. When ignoreIgnoredMigrations is enabled, such case
# will not be reported by validate command. This is useful for situations where one must be able to deliver
# complete set of migrations in a delivery package for multiple versions of the product, and allows for further
# development of older versions.
# true to continue normally, false to fail fast with an exception. (default: false)
# flyway.ignoreIgnoredMigrations=

# Ignore pending migrations when reading the schema history table. These are migrations that are available
# but have not yet been applied. This can be useful for verifying that in-development migration changes
# don't contain any validation-breaking changes of migrations that have already been applied to a production
# environment, e.g. as part of a CI/CD process, without failing because of the existence of new migration versions.
# (default: false)
# flyway.ignorePendingMigrations=

# Ignore future migrations when reading the schema history table. These are migrations that were performed by a
# newer deployment of the application that are not yet available in this version. For example: we have migrations
# available on the classpath up to version 3.0. The schema history table indicates that a migration to version 4.0
# (unknown to us) has already been applied. Instead of bombing out (fail fast) with an exception, a
# warning is logged and Flyway continues normally. This is useful for situations where one must be able to redeploy
# an older version of the application after the database has been migrated by a newer one.
# true to continue normally and log a warning, false to fail fast with an exception. (default: true)
# flyway.ignoreFutureMigrations=

# Whether to allow mixing transactional and non-transactional statements within the same migration. Enabling this
# automatically causes the entire affected migration to be run without a transaction.
# Note that this is only applicable for PostgreSQL, Aurora PostgreSQL, SQL Server and SQLite which all have
# statements that do not run at all within a transaction.
# This is not to be confused with implicit transaction, as they occur in MySQL or Oracle, where even though a
# DDL statement was run within within a transaction, the database will issue an implicit commit before and after
# its execution.
# true if mixed migrations should be allowed. false if an error should be thrown instead. (default: false)
# flyway.mixed=

# Whether to group all pending migrations together in the same transaction when applying them
# (only recommended for databases with support for DDL transactions).
# true if migrations should be grouped. false if they should be applied individually instead. (default: false)
# flyway.group=

# The username that will be recorded in the schema history table as having applied the migration.
# <<blank>> for the current database user of the connection. (default: <<blank>>).
# flyway.installedBy=

# Rules for the built-in error handler that let you override specific SQL states and errors codes in order to
# force specific errors or warnings to be treated as debug messages, info messages, warnings or errors.
# Each error override has the following format: STATE:12345:W.
# It is a 5 character SQL state (or * to match all SQL states), a colon,
# the SQL error code (or * to match all SQL error codes), a colon and finally
# the desired behavior that should override the initial one.
# The following behaviors are accepted:
# - D to force a debug message
# - D- to force a debug message, but do not show the original sql state and error code
# - I to force an info message
# - I- to force an info message, but do not show the original sql state and error code
# - W to force a warning
# - W- to force a warning, but do not show the original sql state and error code
# - E to force an error
# - E- to force an error, but do not show the original sql state and error code
# Example 1: to force Oracle stored procedure compilation issues to produce
# errors instead of warnings, the following errorOverride can be used: 99999:17110:E
# Example 2: to force SQL Server PRINT messages to be displayed as info messages (without SQL state and error
# code details) instead of warnings, the following errorOverride can be used: S0001:0:I-
# Example 3: to force all errors with SQL error code 123 to be treated as warnings instead,
# the following errorOverride can be used: *:123:W
# Flyway Pro and Flyway Enterprise only
# flyway.errorOverrides=

# The file where to output the SQL statements of a migration dry run. If the file specified is in a non-existent
# directory, Flyway will create all directories and parent directories as needed.
# <<blank>> to execute the SQL statements directly against the database. (default: <<blank>>)
# Flyway Pro and Flyway Enterprise only
# flyway.dryRunOutput=

# Whether to Flyway's support for Oracle SQL*Plus commands should be activated. (default: false)
# Flyway Pro and Flyway Enterprise only
# flyway.oracle.sqlplus=

# Whether Flyway should issue a warning instead of an error whenever it encounters an Oracle SQL*Plus
# statement it doesn't yet support. (default: false)
# Flyway Pro and Flyway Enterprise only
# flyway.oracle.sqlplusWarn=

# Your Flyway license key (FL01...). Not yet a Flyway Pro or Enterprise Edition customer?
# Request your Flyway trial license key st https://flywaydb.org/download/
# to try out Flyway Pro and Enterprise Edition features free for 30 days.
# Flyway Pro and Flyway Enterprise only
# flyway.licenseKey=
```

<p class="next-steps">
    <a class="btn btn-primary" href="/documentation/articles">Articles <i class="fa fa-arrow-right"></i></a>
</p>