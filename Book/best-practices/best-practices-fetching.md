### 25.6. Fetching

    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Fetching too much data is the number one performance issue for the vast majority of JPA applications.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Hibernate supports both entity queries (JPQL/HQL and Criteria API) and native SQL statements.
    Entity queries are useful only if you need to modify the fetched entities, therefore benefiting from the automatic dirty checking mechanism.

    </div>
    <div class="paragraph">

    For read-only transactions, you should fetch DTO projections because they allow you to select just as many columns as you need to fulfill a certain business use case.
    This has many benefits like reducing the load on the currently running Persistence Context because DTO projections don&#8217;t need to be managed.

    </div>
    <div class="sect3">