 ### 4.2. Database objects

    <div class="paragraph">

    Hibernate allows you to customize the schema generation process via the HBM `database-object` element.

    </div>
    <div class="paragraph">

    Considering the following HBM mapping:

    </div>
    <div id="schema-generation-database-object-example" class="exampleblock">
    <div class="title">Example 216. Schema generation HBM database-object</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;?xml version="1.0"?&gt;
    &lt;!DOCTYPE hibernate-mapping PUBLIC
            "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
            "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd" &gt;

    &lt;hibernate-mapping&gt;
        &lt;database-object&gt;
            &lt;create&gt;
                CREATE OR REPLACE FUNCTION sp_count_books(
                    IN authorId bigint,
                    OUT bookCount bigint)
                    RETURNS bigint AS
                $BODY$
                    BEGIN
                        SELECT COUNT(*) INTO bookCount
                        FROM book
                        WHERE author_id = authorId;
                    END;
                $BODY$
                LANGUAGE plpgsql;
            &lt;/create&gt;
            &lt;drop&gt;&lt;/drop&gt;
            &lt;dialect-scope name="org.hibernate.dialect.PostgreSQL95Dialect" /&gt;
        &lt;/database-object&gt;
    &lt;/hibernate-mapping&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When the `SessionFactory` is bootstrapped, Hibernate is going to execute the `database-object`, therefore creating the `sp_count_books` function.

    </div>
    </div>
    <div class="sect2">