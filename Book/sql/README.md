 ## 17. Native SQL Queries

    <div class="sectionbody">
    <div class="paragraph">

    You may also express queries in the native SQL dialect of your database.
    This is useful if you want to utilize database specific features such as window functions, Common Table Expressions (CTE) or the `CONNECT BY` option in Oracle.
    It also provides a clean migration path from a direct SQL/JDBC based application to Hibernate/JPA.
    Hibernate also allows you to specify handwritten SQL (including stored procedures) for all create, update, delete, and retrieve operations.

    </div>
    <div class="sect2">