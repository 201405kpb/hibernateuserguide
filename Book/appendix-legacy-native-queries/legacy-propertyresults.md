 ### 30.2. Legacy return-property to explicitly specify column/alias names

    <div class="paragraph">

    You can explicitly tell Hibernate what column aliases to use with `&lt;return-property&gt;`, instead of using the `{}` syntax to let Hibernate inject its own aliases.
    For example:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;sql-query name = "mySqlQuery"&gt;
        &lt;return alias = "person" class = "eg.Person"&gt;
            &lt;return-property name = "name" column = "myName"/&gt;
            &lt;return-property name = "age" column = "myAge"/&gt;
            &lt;return-property name = "sex" column = "mySex"/&gt;
        &lt;/return&gt;
        SELECT person.NAME AS myName,
               person.AGE AS myAge,
               person.SEX AS mySex,
        FROM PERSON person WHERE person.NAME LIKE :name
    &lt;/sql-query&gt;`</pre>
    </div>
    </div>
    <div class="paragraph">

    `&lt;return-property&gt;` also works with multiple columns.
    This solves a limitation with the `{}` syntax which cannot allow fine grained control of multi-column properties.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;sql-query name = "organizationCurrentEmployments"&gt;
        &lt;return alias = "emp" class = "Employment"&gt;
            &lt;return-property name = "salary"&gt;
                &lt;return-column name = "VALUE"/&gt;
                &lt;return-column name = "CURRENCY"/&gt;
            &lt;/return-property&gt;
            &lt;return-property name = "endDate" column = "myEndDate"/&gt;
        &lt;/return&gt;
            SELECT EMPLOYEE AS {emp.employee}, EMPLOYER AS {emp.employer},
            STARTDATE AS {emp.startDate}, ENDDATE AS {emp.endDate},
            REGIONCODE as {emp.regionCode}, EID AS {emp.id}, VALUE, CURRENCY
            FROM EMPLOYMENT
            WHERE EMPLOYER = :id AND ENDDATE IS NULL
            ORDER BY STARTDATE ASC
    &lt;/sql-query&gt;`</pre>
    </div>
    </div>
    <div class="paragraph">

    In this example `&lt;return-property&gt;` was used in combination with the `{}` syntax for injection.
    This allows users to choose how they want to refer column and properties.

    </div>
    <div class="paragraph">

    If your mapping has a discriminator you must use `&lt;return-discriminator&gt;` to specify the discriminator column.

    </div>
    </div>
    <div class="sect2">