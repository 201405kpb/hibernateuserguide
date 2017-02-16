 ### 30.1. Legacy Named SQL queries

    <div class="paragraph">

    Named SQL queries can also be defined during mapping and called in exactly the same way as a named HQL query.
    In this case, you do _not_ need to call `addEntity()` anymore.

    </div>
    <div class="exampleblock">
    <div class="title">Example 503. Named sql query using the `&lt;sql-query&gt;` mapping element</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;sql-query name = "persons"&gt;
        &lt;return alias="person" class="eg.Person"/&gt;
        SELECT person.NAME AS {person.name},
               person.AGE AS {person.age},
               person.SEX AS {person.sex}
        FROM PERSON person
        WHERE person.NAME LIKE :namePattern
    &lt;/sql-query&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="exampleblock">
    <div class="title">Example 504. Execution of a named query</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List people = session
        .getNamedQuery( "persons" )
        .setParameter( "namePattern", namePattern )
        .setMaxResults( 50 )
        .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `&lt;return-join&gt;` element is use to join associations and the `&lt;load-collection&gt;` element is used to define queries which initialize collections.

    </div>
    <div class="exampleblock">
    <div class="title">Example 505. Named sql query with association</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;sql-query name = "personsWith"&gt;
        &lt;return alias="person" class="eg.Person"/&gt;
        &lt;return-join alias="address" property="person.mailingAddress"/&gt;
        SELECT person.NAME AS {person.name},
               person.AGE AS {person.age},
               person.SEX AS {person.sex},
               address.STREET AS {address.street},
               address.CITY AS {address.city},
               address.STATE AS {address.state},
               address.ZIP AS {address.zip}
        FROM PERSON person
        JOIN ADDRESS address
            ON person.ID = address.PERSON_ID AND address.TYPE='MAILING'
        WHERE person.NAME LIKE :namePattern
    &lt;/sql-query&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    A named SQL query may return a scalar value.
    You must declare the column alias and Hibernate type using the `&lt;return-scalar&gt;` element:

    </div>
    <div class="exampleblock">
    <div class="title">Example 506. Named query returning a scalar</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;sql-query name = "mySqlQuery"&gt;
        &lt;return-scalar column = "name" type="string"/&gt;
        &lt;return-scalar column = "age" type="long"/&gt;
        SELECT p.NAME AS name,
               p.AGE AS age,
        FROM PERSON p WHERE p.NAME LIKE 'Hiber%'
    &lt;/sql-query&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    You can externalize the resultset mapping information in a `&lt;resultset&gt;` element which will allow you to either reuse them across several named queries or through the `setResultSetMapping()` API.

    </div>
    <div class="exampleblock">
    <div class="title">Example 507. &lt;resultset&gt; mapping used to externalize mappinginformation</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;resultset name = "personAddress"&gt;
        &lt;return alias="person" class="eg.Person"/&gt;
        &lt;return-join alias="address" property="person.mailingAddress"/&gt;
    &lt;/resultset&gt;

    &lt;sql-query name = "personsWith" resultset-ref="personAddress"&gt;
        SELECT person.NAME AS {person.name},
               person.AGE AS {person.age},
               person.SEX AS {person.sex},
               address.STREET AS {address.street},
               address.CITY AS {address.city},
               address.STATE AS {address.state},
               address.ZIP AS {address.zip}
        FROM PERSON person
        JOIN ADDRESS address
            ON person.ID = address.PERSON_ID AND address.TYPE='MAILING'
        WHERE person.NAME LIKE :namePattern
    &lt;/sql-query&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    You can, alternatively, use the resultset mapping information in your hbm files directly in java code.

    </div>
    <div class="exampleblock">
    <div class="title">Example 508. Programmatically specifying the result mapping information</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = session
        .createSQLQuery( "select {cat.*}, {kitten.*} from cats cat, cats kitten where kitten.mother = cat.id" )
        .setResultSetMapping("catAndKitten")
        .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">