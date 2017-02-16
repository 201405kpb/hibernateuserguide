### 10.6. JPA locking query hints

    <div class="paragraph">

    JPA 2.0 introduced two query hints:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">javax.persistence.lock.timeout</dt>
    <dd>

    it gives the number of milliseconds a lock acquisition request will wait before throwing an exception

    </dd>
    <dt class="hdlist1">javax.persistence.lock.scope</dt>
    <dd>

    defines the [_scope_](http://docs.oracle.com/javaee/7/api/javax/persistence/PessimisticLockScope.html) of the lock acquisition request.
    The scope can either be `NORMAL` (default value) or `EXTENDED`. The `EXTENDED` scope will cause a lock acquisition request to be passed to other owned table structured (e.g. `@Inheritance(strategy=InheritanceType.JOINED)`, `@ElementCollection`)

    </dd>
    </dl>
    </div>
    <div id="locking-jpa-query-hints-timeout-example" class="exampleblock">
    <div class="title">Example 278. `javax.persistence.lock.timeout` example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`entityManager.find(
        Person.class, id, LockModeType.PESSIMISTIC_WRITE,
        Collections.singletonMap( "javax.persistence.lock.timeout", 200 )
    );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT explicitlo0_.id     AS id1_0_0_,
           explicitlo0_."name" AS name2_0_0_
    FROM   person explicitlo0_
    WHERE  explicitlo0_.id = 1
    FOR UPDATE wait 2`</pre>
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

    Not all JDBC database drivers support setting a timeout value for a locking request.
    If not supported, the Hibernate dialect ignores this query hint.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The `javax.persistence.lock.scope` is [not yet supported](https://hibernate.atlassian.net/browse/HHH-9636) as specified by the JPA standard.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">