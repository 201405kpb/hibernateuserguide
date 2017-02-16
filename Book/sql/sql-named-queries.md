 ### 17.10. Named SQL queries

    <div class="paragraph">

    Named SQL queries can also be defined during mapping and called in exactly the same way as a named HQL query.
    In this case, you do _not_ need to call `addEntity()` anymore.

    </div>
    <div class="paragraph">

    JPA defines the `javax.persistence.NamedNativeQuery` annotation for this purpose,
    and the Hibernate `org.hibernate.annotations.NamedNativeQuery` annotation extends it and adds the following attributes:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`flushMode()`</dt>
    <dd>

    The flush mode for the query. By default, it uses the current Persistence Context flush mode.

    </dd>
    <dt class="hdlist1">`cacheable()`</dt>
    <dd>

    Whether the query (results) is cacheable or not. By default, queries are not cached.

    </dd>
    <dt class="hdlist1">`cacheRegion()`</dt>
    <dd>

    If the query results are cacheable, name the query cache region to use.

    </dd>
    <dt class="hdlist1">`fetchSize()`</dt>
    <dd>

    The number of rows fetched by the JDBC Driver per database trip. The default value is given by the JDBC driver.

    </dd>
    <dt class="hdlist1">`timeout()`</dt>
    <dd>

    The query timeout (in seconds). By default, there&#8217;s no timeout.

    </dd>
    <dt class="hdlist1">`callable()`</dt>
    <dd>

    Does the SQL query represent a call to a procedure/function? Default is false.

    </dd>
    <dt class="hdlist1">`comment()`</dt>
    <dd>

    A comment added to the SQL query for tuning the execution plan.

    </dd>
    <dt class="hdlist1">`cacheMode()`</dt>
    <dd>

    The cache mode used for this query. This refers to entities/collections returned by the query.
    The default value is `CacheModeType.NORMAL`.

    </dd>
    <dt class="hdlist1">`readOnly()`</dt>
    <dd>

    Whether the results should be read-only. By default, queries are not read-only so entities are stored in the Persistence Context.

    </dd>
    </dl>
    </div>
    <div class="sect3">