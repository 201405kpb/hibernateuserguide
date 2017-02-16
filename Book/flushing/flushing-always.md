 ### 6.3. `ALWAYS` flush

    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The `ALWAYS` is only available with the native `Session` API.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The `ALWAYS` flush mode triggers a persistence context flush even when executing a native SQL query against the `Session` API.

    </div>
    <div id="flushing-always-flush-sql-example" class="exampleblock">
    <div class="title">Example 270. `COMMIT` flushing on SQL</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person("John Doe");
    entityManager.persist(person);

    Session session = entityManager.unwrap( Session.class);
    assertTrue(((Number) session
            .createSQLQuery("select count(*) from Person")
            .setFlushMode( FlushMode.ALWAYS)
            .uniqueResult()).intValue() == 1);`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Person (name, id) VALUES ('John Doe', 1)

    SELECT COUNT(*) FROM Person`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">