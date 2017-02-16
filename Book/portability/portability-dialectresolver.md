 ### 22.3. Dialect resolution

    <div class="paragraph">

    Originally, Hibernate would always require that users specify which dialect to use. In the case of users looking to simultaneously target multiple databases with their build that was problematic.
    Generally, this required their users to configure the Hibernate dialect or defining their own method of setting that value.

    </div>
    <div class="paragraph">

    Starting with version 3.2, Hibernate introduced the notion of automatically detecting the dialect to use based on the `java.sql.DatabaseMetaData` obtained from a `java.sql.Connection` to that database.
    This was much better, expect that this resolution was limited to databases Hibernate know about ahead of time and was in no way configurable or overrideable.

    </div>
    <div class="paragraph">

    Starting with version 3.3, Hibernate has a fare more powerful way to automatically determine which dialect to should be used by relying on a series of delegates which implement the `org.hibernate.dialect.resolver.DialectResolver` which defines only a single method:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public Dialect resolveDialect(DatabaseMetaData metaData) throws JDBCConnectionException`</pre>
    </div>
    </div>
    <div class="paragraph">

    The basic contract here is that if the resolver 'understands' the given database metadata then it returns the corresponding Dialect; if not it returns null and the process continues to the next resolver.
    The signature also identifies `org.hibernate.exception.JDBCConnectionException` as possibly being thrown.
    A `JDBCConnectionException` here is interpreted to imply a "non transient" (aka non-recoverable) connection problem and is used to indicate an immediate stop to resolution attempts.
    All other exceptions result in a warning and continuing on to the next resolver.

    </div>
    <div class="paragraph">

    The cool part about these resolvers is that users can also register their own custom resolvers which will be processed ahead of the built-in Hibernate ones.
    This might be useful in a number of different situations:

    </div>
    <div class="ulist">

*   it allows easy integration for auto-detection of dialects beyond those shipped with Hibernate itself
*   it allows you to specify to use a custom dialect when a particular database is recognized.
    </div>
    <div class="paragraph">

    To register one or more resolvers, simply specify them (separated by commas, tabs or spaces) using the 'hibernate.dialect_resolvers' configuration setting (see the `DIALECT_RESOLVERS` constant on `org.hibernate.cfg.Environment`).

    </div>
    </div>
    <div class="sect2">