 #### 19.2.1. Separate database

    <div class="paragraph">

    <span class="image">![multitenacy_database](images/multitenancy/multitenacy_database.png)</span>

    </div>
    <div class="paragraph">

    Each tenant&#8217;s data is kept in a physically separate database instance.
    JDBC Connections would point specifically to each database so any pooling would be per-tenant.
    A general application approach, here, would be to define a JDBC Connection pool per-tenant and to select the pool to use based on the _tenant identifier_ associated with the currently logged in user.

    </div>
    </div>
    <div class="sect3">
