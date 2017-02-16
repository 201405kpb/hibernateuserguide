 ### 15.44. In predicate

    <div class="paragraph">

    `IN` predicates performs a check that a particular value is in a list of values. Its syntax is:

    </div>
    <div id="hql-in-predicate-bnf" class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`in_expression ::=
        single_valued_expression [NOT] IN single_valued_list

    single_valued_list ::=
        constructor_expression | (subquery) | collection_valued_input_parameter

    constructor_expression ::= (expression[, expression]*)`</pre>
    </div>
    </div>
    <div class="paragraph">

    The types of the `single_valued_expression` and the individual values in the `single_valued_list` must be consistent.

    </div>
    <div class="paragraph">

    JPQL limits the valid types here to string, numeric, date, time, timestamp, and enum types, and , in JPQL, `single_valued_expression` can only refer to:

    </div>
    <div class="ulist">

*   "state fields", which is its term for simple attributes. Specifically this excludes association and component/embedded attributes.
*   entity type expressions. See [Entity type](#hql-entity-type-exp)
    </div>
    <div class="paragraph">

    In HQL, `single_valued_expression` can refer to a far more broad set of expression types.
    Single-valued association are allowed, and so are component/embedded attributes, although that feature depends on the level of support for tuple or "row value constructor syntax" in the underlying database.
    Additionally, HQL does not limit the value type in any way, though application developers should be aware that different types may incur limited support based on the underlying database vendor.
    This is largely the reason for the JPQL limitations.

    </div>
    <div class="paragraph">

    The list of values can come from a number of different sources.
    In the `constructor_expression` and `collection_valued_input_parameter`, the list of values must not be empty; it must contain at least one value.

    </div>
    <div id="hql-in-predicate-example" class="exampleblock">
    <div class="title">Example 401. In predicate examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Payment&gt; payments = entityManager.createQuery(
        "select p " +
        "from Payment p " +
        "where type(p) in ( CreditCardPayment, WireTransferPayment )", Payment.class )
    .getResultList();

    List&lt;Phone&gt; phones = entityManager.createQuery(
        "select p " +
        "from Phone p " +
        "where type in ( 'MOBILE', 'LAND_LINE' )", Phone.class )
    .getResultList();

    List&lt;Phone&gt; phones = entityManager.createQuery(
        "select p " +
        "from Phone p " +
        "where type in :types", Phone.class )
    .setParameter( "types", Arrays.asList( PhoneType.MOBILE, PhoneType.LAND_LINE ) )
    .getResultList();

    List&lt;Phone&gt; phones = entityManager.createQuery(
        "select distinct p " +
        "from Phone p " +
        "where p.person.id in (" +
        "    select py.person.id " +
        "    from Payment py" +
        "    where py.completed = true and py.amount &gt; 50 " +
        ")", Phone.class )
    .getResultList();

    // Not JPQL compliant!
    List&lt;Phone&gt; phones = entityManager.createQuery(
        "select distinct p " +
        "from Phone p " +
        "where p.person in (" +
        "    select py.person " +
        "    from Payment py" +
        "    where py.completed = true and py.amount &gt; 50 " +
        ")", Phone.class )
    .getResultList();

    // Not JPQL compliant!
    List&lt;Payment&gt; payments = entityManager.createQuery(
        "select distinct p " +
        "from Payment p " +
        "where ( p.amount, p.completed ) in (" +
        "    (50, true )," +
        "    (100, true )," +
        "    (5, false )" +
        ")", Payment.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">