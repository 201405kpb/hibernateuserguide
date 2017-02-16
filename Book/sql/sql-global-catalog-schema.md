### 17.11. Resolving global catalog and schema in native SQL queries

    <div class="paragraph">

    When using multiple database catalogs and schemas, Hibernate offers the possibility of
    setting a global catalog or schema so that you don&#8217;t have to declare it explicitly for every entity.

    </div>
    <div id="sql-global-catalog-schema-configuration" class="exampleblock">
    <div class="title">Example 465. Setting global catalog and schema</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;property name="hibernate.default_catalog" value="crm"/&gt;
    &lt;property name="hibernate.default_schema" value="analytics"/&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This way, we can imply the global **crm** catalog and **analytics** schema in every JPQL, HQL or Criteria API query.

    </div>
    <div class="paragraph">

    However, for native queries, the SQL query is passed as is, therefore you need to explicitly set the global catalog and schema whenever you are referencing a database table.
    Fortunately, Hibernate allows you to resolve the current global catalog and schema using the following placeholders:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">{h-catalog}</dt>
    <dd>

    resolves the current `hibernate.default_catalog` configuration property value.

    </dd>
    <dt class="hdlist1">{h-schema}</dt>
    <dd>

    resolves the current `hibernate.default_schema` configuration property value.

    </dd>
    <dt class="hdlist1">{h-domain}</dt>
    <dd>

    resolves the current `hibernate.default_catalog` and `hibernate.default_schema` configuration property values (e.g. catalog.schema).

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    Withe these placeholders, you can imply the catalog, schema, or both catalog and schema for every native query.

    </div>
    <div class="paragraph">

    So, when running the following native query:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@NamedNativeQuery(
        name = "last_30_days_hires",
        query =
            "select * " +
            "from {h-domain}person " +
            "where age(hired_on) &lt; '30 days'",
        resultClass = Person.class
    )`</pre>
    </div>
    </div>
    <div class="paragraph">

    Hibernate is going to resolve the `{h-domain}` placeholder according to the values of the default catalog and schema:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT *
    FROM   crm.analytics.person
    WHERE  age(hired_on) &lt; '30 days'`</pre>
    </div>
    </div>
    </div>
    <div class="sect2">