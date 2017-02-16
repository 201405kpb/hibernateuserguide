#### 19.2.2. Separate schema

    <div class="paragraph">

    <span class="image">![multitenacy_schema](images/multitenancy/multitenacy_schema.png)</span>

    </div>
    <div class="paragraph">

    Each tenant&#8217;s data is kept in a distinct database schema on a single database instance.
    There are two different ways to define JDBC Connections here:

    </div>
    <div class="ulist">

*   Connections could point specifically to each schema as we saw with the `Separate database` approach.
    This is an option provided that the driver supports naming the default schema in the connection URL or if the pooling mechanism supports naming a schema to use for its Connections.
    Using this approach, we would have a distinct JDBC Connection pool per-tenant where the pool to use would be selected based on the "tenant identifier" associated with the currently logged in user.
*   Connections could point to the database itself (using some default schema) but the Connections would be altered using the SQL `SET SCHEMA` (or similar) command.
    Using this approach, we would have a single JDBC Connection pool for use to service all tenants, but before using the Connection, it would be altered to reference the schema named by the "tenant identifier" associated with the currently logged in user.
    </div>
    </div>
    </div>
    <div class="sect2">