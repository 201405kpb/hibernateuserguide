 ### 6.4. `MANUAL` flush

    <div class="paragraph">

    Both the `EntityManager` and the Hibernate `Session` define a `flush()` method that, when called, triggers a manual flush.
    Hibernate also defines a `MANUAL` flush mode so the persistence context can only be flushed manually.

    </div>
    <div id="flushing-manual-flush-example" class="exampleblock">
    <div class="title">Example 271. `MANUAL` flushing</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person("John Doe");
    entityManager.persist(person);

    Session session = entityManager.unwrap( Session.class);
    session.setHibernateFlushMode( FlushMode.MANUAL );

    assertTrue(((Number) entityManager
        .createQuery("select count(id) from Person")
        .getSingleResult()).intValue() == 0);

    assertTrue(((Number) session
        .createSQLQuery("select count(*) from Person")
        .uniqueResult()).intValue() == 0);`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT COUNT(p.id) AS col_0_0_
    FROM   Person p

    SELECT COUNT(*)
    FROM   Person`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `INSERT` statement was not executed because the persistence context because there was no manual `flush()` call.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    This mode is useful when using multi-request logical transactions and only the last request should flush the persistence context.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">