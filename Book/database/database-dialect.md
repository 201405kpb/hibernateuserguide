 ### 7.12. Database Dialect

    <div class="paragraph">

    Although SQL is relatively standardized, each database vendor uses a subset and superset of ANSI SQL defined syntax.
    This is referred to as the database&#8217;s dialect.
    Hibernate handles variations across these dialects through its `org.hibernate.dialect.Dialect` class and the various subclasses for each database vendor.

    </div>
    <div class="paragraph">

    In most cases Hibernate will be able to determine the proper Dialect to use by asking some questions of the JDBC Connection during bootstrap.
    For information on Hibernate&#8217;s ability to determine the proper Dialect to use (and your ability to influence that resolution), see [Dialect resolution](#portability-dialectresolver).

    </div>
    <div class="paragraph">

    If for some reason it is not able to determine the proper one or you want to use a custom Dialect, you will need to set the `hibernate.dialect` setting.

    </div>
    <table class="tableblock frame-all grid-all spread">
    <caption class="title">Table 4. Provided Dialects</caption>
    <colgroup>
    <col style="width: 28%;">
    <col style="width: 72%;">
    </colgroup>
    <thead>
    <tr>
    <th class="tableblock halign-left valign-top">Dialect (short name)</th>
    <th class="tableblock halign-left valign-top">Remarks</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    Cache71
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Caché database, version 2007.1
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    CUBRID
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the CUBRID database, version 8.3. May work with later versions.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    DB2
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the DB2 database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    DB2390
    </td>
    <td class="tableblock halign-left valign-top">

    Support for DB2 Universal Database for OS/390, also known as DB2/390.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    DB2400
    </td>
    <td class="tableblock halign-left valign-top">

    Support for DB2 Universal Database for iSeries, also known as DB2/400.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    DerbyTenFive
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Derby database, version 10.5
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    DerbyTenSix
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Derby database, version 10.6
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    DerbyTenSeven
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Derby database, version 10.7
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Firebird
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Firebird database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    FrontBase
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Frontbase database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    H2
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the H2 database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    HSQL
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the HSQL (HyperSQL) database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Informix
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Informix database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Ingres
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Ingres database, version 9.2
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Ingres9
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Ingres database, version 9.3. May work with newer versions
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Ingres10
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Ingres database, version 10. May work with newer versions
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Interbase
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Interbase database.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    JDataStore
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the JDataStore database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    McKoi
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the McKoi database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Mimer
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Mimer database, version 9.2.1. May work with newer versions
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    MySQL5
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the MySQL database, version 5.x
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    MySQL5InnoDB
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the MySQL database, version 5.x preferring the InnoDB storage engine when exporting tables.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    MySQL57InnoDB
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the MySQL database, version 5.7 preferring the InnoDB storage engine when exporting tables. May work with newer versions
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Oracle8i
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Oracle database, version 8i
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Oracle9i
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Oracle database, version 9i
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Oracle10g
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Oracle database, version 10g
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Pointbase
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Pointbase database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    PostgresPlus
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Postgres Plus database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    PostgreSQL81
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the PostgrSQL database, version 8.1
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    PostgreSQL82
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the PostgreSQL database, version 8.2
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    PostgreSQL9
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the PostgreSQL database, version 9. May work with later versions.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Progress
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Progress database, version 9.1C. May work with newer versions.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    SAPDB
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the SAPDB/MAXDB database.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    SQLServer
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the SQL Server 2000 database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    SQLServer2005
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the SQL Server 2005 database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    SQLServer2008
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the SQL Server 2008 database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Sybase11
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Sybase database, up to version 11.9.2
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    SybaseAnywhere
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Sybase Anywhere database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    SybaseASE15
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Sybase Adaptive Server Enterprise database, version 15
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    SybaseASE157
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Sybase Adaptive Server Enterprise database, version 15.7. May work with newer versions.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Teradata
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the Teradata database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    TimesTen
    </td>
    <td class="tableblock halign-left valign-top">

    Support for the TimesTen database, version 5.1. May work with newer versions
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    </div>
    </div>
    <div class="sect1">