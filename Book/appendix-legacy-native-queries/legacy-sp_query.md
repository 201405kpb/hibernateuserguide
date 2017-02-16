### 30.3. Legacy stored procedures for querying

    <div class="paragraph">

    Hibernate provides support for queries via stored procedures and functions.
    Most of the following documentation is equivalent for both.
    The stored procedure/function must return a resultset as the first out-parameter to be able to work with Hibernate.
    An example of such a stored function in Oracle 9 and higher is as follows:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE OR REPLACE FUNCTION selectAllEmployments
        RETURN SYS_REFCURSOR
    AS
        st_cursor SYS_REFCURSOR;
    BEGIN
        OPEN st_cursor FOR
            SELECT EMPLOYEE, EMPLOYER,
            STARTDATE, ENDDATE,
            REGIONCODE, EID, VALUE, CURRENCY
            FROM EMPLOYMENT;
        RETURN  st_cursor;
    END;`</pre>
    </div>
    </div>
    <div class="paragraph">

    To use this query in Hibernate you need to map it via a named query.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;sql-query name = "selectAllEmployees_SP" callable = "true"&gt;
        &lt;return alias="emp" class="Employment"&gt;
            &lt;return-property name = "employee" column = "EMPLOYEE"/&gt;
            &lt;return-property name = "employer" column = "EMPLOYER"/&gt;
            &lt;return-property name = "startDate" column = "STARTDATE"/&gt;
            &lt;return-property name = "endDate" column = "ENDDATE"/&gt;
            &lt;return-property name = "regionCode" column = "REGIONCODE"/&gt;
            &lt;return-property name = "id" column = "EID"/&gt;
            &lt;return-property name = "salary"&gt;
                &lt;return-column name = "VALUE"/&gt;
                &lt;return-column name = "CURRENCY"/&gt;
            &lt;/return-property&gt;
        &lt;/return&gt;
        { ? = call selectAllEmployments() }
    &lt;/sql-query&gt;`</pre>
    </div>
    </div>
    <div class="paragraph">

    Stored procedures currently only return scalars and entities.
    `&lt;return-join&gt;` and `&lt;load-collection&gt;` are not supported.

    </div>
    </div>
    <div class="sect2">