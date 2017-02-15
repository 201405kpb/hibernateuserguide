   #### 2.8.9. Arrays

    <div class="paragraph">

    When it comes to arrays, there is quite a difference between Java arrays and relational database array types (e.g. VARRAY, ARRAY).
    First, not all database systems implement the SQL-99 ARRAY type, and, for this reason, Hibernate doesn&#8217;t support native database array types.
    Second, Java arrays are relevant for basic types only since storing multiple embeddables or entities should always be done using the Java Collection API.

    </div>
    </div>
    <div class="sect3">