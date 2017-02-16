### 15.24. Arithmetic

    <div class="paragraph">

    Arithmetic operations also represent valid expressions.

    </div>
    <div id="hql-numeric-arithmetic-example" class="exampleblock">
    <div class="title">Example 383. Numeric arithmetic examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`// select clause date/time arithmetic operations
    Long duration = entityManager.createQuery(
        "select sum(ch.duration) * :multiplier " +
        "from Person pr " +
        "join pr.phones ph " +
        "join ph.callHistory ch " +
        "where ph.id = 1L ", Long.class )
    .setParameter( "multiplier", 1000L )
    .getSingleResult();

    // select clause date/time arithmetic operations
    Integer years = entityManager.createQuery(
        "select year( current_date() ) - year( p.createdOn ) " +
        "from Person p " +
        "where p.id = 1L", Integer.class )
    .getSingleResult();

    // where clause arithmetic operations
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where year( current_date() ) - year( p.createdOn ) &gt; 1", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The following rules apply to the result of arithmetic operations:

    </div>
    <div class="ulist">

*   If either of the operands is `Double`/`double`, the result is a `Double`
*   else, if either of the operands is `Float`/`float`, the result is a `Float`
*   else, if either operand is `BigDecimal`, the result is `BigDecimal`
*   else, if either operand is `BigInteger`, the result is `BigInteger` (except for division, in which case the result type is not further defined)
*   else, if either operand is `Long`/`long`, the result is `Long` (except for division, in which case the result type is not further defined)
*   else, (the assumption being that both operands are of integral type) the result is `Integer` (except for division, in which case the result type is not further defined)
    </div>
    <div class="paragraph">

    Date arithmetic is also supported, albeit in a more limited fashion.
    This is due partially to differences in database support and partially to the lack of support for `INTERVAL` definition in the query language itself.

    </div>
    </div>
    <div class="sect2">