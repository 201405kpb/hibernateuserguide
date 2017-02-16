 ### 10.8. Follow-on-locking

    <div class="paragraph">

    When using Oracle, the [`FOR UPDATE` exclusive locking clause](https://docs.oracle.com/database/121/SQLRF/statements_10002.htm#SQLRF55371) cannot be used with:

    </div>
    <div class="ulist">

*   `DISTINCT`
*   `GROUP BY`
*   `UNION`
*   inlined views (derived tables), therefore, affecting the legacy Oracle pagination mechanism as well.
    </div>
    <div class="paragraph">

    For this reason, Hibernate uses secondary selects to lock the previously fetched entities.

    </div>
    <div id="locking-follow-on-example" class="exampleblock">
    <div class="title">Example 280. Follow-on-locking example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select DISTINCT p from Person p", Person.class)
    .setLockMode( LockModeType.PESSIMISTIC_WRITE )
    .getResultList();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT DISTINCT p.id as id1_0_, p."name" as name2_0_
    FROM Person p

    SELECT id
    FROM Person
    WHERE id = 1 FOR UPDATE

    SELECT id
    FROM Person
    WHERE id = 1 FOR UPDATE`</pre>
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

    To avoid the N+1 query problem, a separate query can be used to apply the lock using the associated entity identifiers.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div id="locking-follow-on-secondary-query-example" class="exampleblock">
    <div class="title">Example 281. Secondary query entity locking</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select DISTINCT p from Person p", Person.class)
    .getResultList();

    entityManager.createQuery(
        "select p.id from Person p where p in :persons")
    .setLockMode( LockModeType.PESSIMISTIC_WRITE )
    .setParameter( "persons", persons )
    .getResultList();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT DISTINCT p.id as id1_0_, p."name" as name2_0_
    FROM Person p

    SELECT p.id as col_0_0_
    FROM Person p
    WHERE p.id IN ( 1 , 2 )
    FOR UPDATE`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The lock request was moved from the original query to a secondary one which takes the previously fetched entities to lock their associated database records.

    </div>
    <div class="paragraph">

    Prior to Hibernate 5.2.1, the the follow-on-locking mechanism was applied uniformly to any locking query executing on Oracle.
    Since 5.2.1, the Oracle Dialect tries to figure out if the current query demand the follow-on-locking mechanism.

    </div>
    <div class="paragraph">

    Even more important is that you can overrule the default follow-on-locking detection logic and explicitly enable or disable it on a per query basis.

    </div>
    <div id="locking-follow-on-explicit-example" class="exampleblock">
    <div class="title">Example 282. Disabling the follow-on-locking mechanism explicitly</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p from Person p", Person.class)
    .setMaxResults( 10 )
    .unwrap( Query.class )
    .setLockOptions(
        new LockOptions( LockMode.PESSIMISTIC_WRITE )
            .setFollowOnLocking( false ) )
    .getResultList();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT *
    FROM (
        SELECT p.id as id1_0_, p."name" as name2_0_
        FROM Person p
    )
    WHERE rownum &lt;= 10
    FOR UPDATE`</pre>
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

    The follow-on-locking mechanism should be explicitly enabled only if the current executing query fails because the `FOR UPDATE` clause cannot be applied, meaning that the Dialect resolving mechanism needs to be further improved.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">
