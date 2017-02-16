### 30.6. Legacy custom SQL for loading

    <div class="paragraph">

    You can also declare your own SQL (or HQL) queries for entity loading.
    As with inserts, updates, and deletes, this can be done at the individual column level as described in
    For columns, see [Column transformers: read and write expressions](#mapping-column-read-and-write) or at the statement level.
    Here is an example of a statement level override:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;sql-query name = "person"&gt;
        &lt;return alias = "pers" class = "Person" lock-mod e= "upgrade"/&gt;
        SELECT NAME AS {pers.name}, ID AS {pers.id}
        FROM PERSON
        WHERE ID=?
        FOR UPDATE
    &lt;/sql-query&gt;`</pre>
    </div>
    </div>
    <div class="paragraph">

    This is just a named query declaration, as discussed earlier. You can reference this named query in a class mapping:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;class name = "Person"&gt;
        &lt;id name = "id"&gt;
            &lt;generator class = "increment"/&gt;
        &lt;/id&gt;
        &lt;property name = "name" not-null = "true"/&gt;
        &lt;loader query-ref = "person"/&gt;
    &lt;/class&gt;`</pre>
    </div>
    </div>
    <div class="paragraph">

    This even works with stored procedures.

    </div>
    <div class="paragraph">

    You can even define a query for collection loading:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;set name = "employments" inverse = "true"&gt;
        &lt;key/&gt;
        &lt;one-to-many class = "Employment"/&gt;
        &lt;loader query-ref = "employments"/&gt;
    &lt;/set&gt;`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;sql-query name = "employments"&gt;
        &lt;load-collection alias = "emp" role = "Person.employments"/&gt;
        SELECT {emp.*}
        FROM EMPLOYMENT emp
        WHERE EMPLOYER = :id
        ORDER BY STARTDATE ASC, EMPLOYEE ASC
    &lt;/sql-query&gt;`</pre>
    </div>
    </div>
    <div class="paragraph">

    You can also define an entity loader that loads a collection by join fetching:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;sql-query name = "person"&gt;
        &lt;return alias = "pers" class = "Person"/&gt;
        &lt;return-join alias = "emp" property = "pers.employments"/&gt;
        SELECT NAME AS {pers.*}, {emp.*}
        FROM PERSON pers
        LEFT OUTER JOIN EMPLOYMENT emp
            ON pers.ID = emp.PERSON_ID
        WHERE ID=?
    &lt;/sql-query&gt;

</div>
</div>
</div>
</div>
</div>
<div class="sect1">