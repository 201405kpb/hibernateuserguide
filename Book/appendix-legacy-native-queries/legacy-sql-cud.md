### 30.5. Legacy custom SQL for create, update and delete

    <div class="paragraph">

    Hibernate can use custom SQL for create, update, and delete operations.
    The SQL can be overridden at the statement level or individual column level.
    This section describes statement overrides.
    For columns, see [Column transformers: read and write expressions](#mapping-column-read-and-write).
    The following example shows how to define custom SQL operations using annotations.

    </div>
    <div class="exampleblock">
    <div class="title">Example 509. Custom CRUD XML</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;class name = "Person"&gt;
        &lt;id name = "id"&gt;
            &lt;generator class = "increment"/&gt;
        &lt;/id&gt;
        &lt;property name = "name" not-null = "true"/&gt;
        &lt;sql-insert&gt;INSERT INTO PERSON (NAME, ID) VALUES ( UPPER(?), ? )&lt;/sql-insert&gt;
        &lt;sql-update&gt;UPDATE PERSON SET NAME=UPPER(?) WHERE ID=?&lt;/sql-update&gt;
        &lt;sql-delete&gt;DELETE FROM PERSON WHERE ID=?&lt;/sql-delete&gt;
    &lt;/class&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    If you expect to call a store procedure, be sure to set the `callable` attribute to `true`, in annotations as well as in xml.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    To check that the execution happens correctly, Hibernate allows you to define one of those three strategies:

    </div>
    <div class="ulist">

*   none: no check is performed: the store procedure is expected to fail upon issues
*   count: use of rowcount to check that the update is successful
*   param: like COUNT but using an output parameter rather that the standard mechanism
    </div>
    <div class="paragraph">

    To define the result check style, use the `check` parameter which is again available in annotations as well as in xml.

    </div>
    <div class="paragraph">

    Last but not least, stored procedures are in most cases required to return the number of rows inserted, updated and deleted.
    Hibernate always registers the first statement parameter as a numeric output parameter for the CUD operations:

    </div>
    <div class="exampleblock">
    <div class="title">Example 510. Stored procedures and their return value</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE OR REPLACE FUNCTION updatePerson (uid IN NUMBER, uname IN VARCHAR2)
        RETURN NUMBER IS
    BEGIN

        update PERSON
        set
            NAME = uname,
        where
            ID = uid;

        return SQL%ROWCOUNT;

    END updatePerson;`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">