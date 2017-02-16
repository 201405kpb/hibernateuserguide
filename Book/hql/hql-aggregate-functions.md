### 15.26. Aggregate functions

    <div class="paragraph">

    Aggregate functions are also valid expressions in HQL and JPQL.
    The semantic is the same as their SQL counterpart.
    The supported aggregate functions are:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`COUNT` (including distinct/all qualifiers)</dt>
    <dd>

    The result type is always `Long`.

    </dd>
    <dt class="hdlist1">`AVG`</dt>
    <dd>

    The result type is always `Double`.

    </dd>
    <dt class="hdlist1">`MIN`</dt>
    <dd>

    The result type is the same as the argument type.

    </dd>
    <dt class="hdlist1">`MAX`</dt>
    <dd>

    The result type is the same as the argument type.

    </dd>
    <dt class="hdlist1">`SUM`</dt>
    <dd>

    The result type of the `SUM()` function depends on the type of the values being summed.
    For integral values (other than `BigInteger`), the result type is `Long`.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    For floating point values (other than `BigDecimal`) the result type is `Double`.
    For `BigInteger` values, the result type is `BigInteger`. For `BigDecimal` values, the result type is `BigDecimal`.

    </div>
    <div id="hql-aggregate-functions-example" class="exampleblock">
    <div class="title">Example 385. Aggregate function examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Object[] callStatistics = entityManager.createQuery(
        "select " +
        "    count(c), " +
        "    sum(c.duration), " +
        "    min(c.duration), " +
        "    max(c.duration), " +
        "    avg(c.duration)  " +
        "from Call c ", Object[].class )
    .getSingleResult();

    Long phoneCount = entityManager.createQuery(
        "select count( distinct c.phone ) " +
        "from Call c ", Long.class )
    .getSingleResult();

    List&lt;Object[]&gt; callCount = entityManager.createQuery(
        "select p.number, count(c) " +
        "from Call c " +
        "join c.phone p " +
        "group by p.number", Object[].class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Aggregations often appear with grouping. For information on grouping see [Group by](#hql-group-by).

    </div>
    </div>
    <div class="sect2">