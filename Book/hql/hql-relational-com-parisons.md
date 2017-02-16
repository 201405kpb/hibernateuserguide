 ### 15.40. Relational comparisons

    <div class="paragraph">

    Comparisons involve one of the comparison operators: `=`, `&gt;`, `&gt;=`, `&lt;`, `&lt;=`, `&lt;&gt;`.
    HQL also defines `!=` as a comparison operator synonymous with `&lt;&gt;`.
    The operands should be of the same type.

    </div>
    <div id="hql-relational-comparisons-example" class="exampleblock">
    <div class="title">Example 395. Relational comparison examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`// numeric comparison
    List&lt;Call&gt; calls = entityManager.createQuery(
        "select c " +
        "from Call c " +
        "where c.duration &lt; 30 ", Call.class )
    .getResultList();

    // string comparison
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like 'John%' ", Person.class )
    .getResultList();

    // datetime comparison
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.createdOn &gt; '1950-01-01' ", Person.class )
    .getResultList();

    // enum comparison
    List&lt;Phone&gt; phones = entityManager.createQuery(
        "select p " +
        "from Phone p " +
        "where p.type = 'MOBILE' ", Phone.class )
    .getResultList();

    // boolean comparison
    List&lt;Payment&gt; payments = entityManager.createQuery(
        "select p " +
        "from Payment p " +
        "where p.completed = true ", Payment.class )
    .getResultList();

    // boolean comparison
    List&lt;Payment&gt; payments = entityManager.createQuery(
        "select p " +
        "from Payment p " +
        "where type(p) = WireTransferPayment ", Payment.class )
    .getResultList();

    // entity value comparison
    List&lt;Object[]&gt; phonePayments = entityManager.createQuery(
        "select p " +
        "from Payment p, Phone ph " +
        "where p.person = ph.person ", Object[].class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Comparisons can also involve subquery qualifiers: `ALL`, `ANY`, `SOME`. `SOME` and `ANY` are synonymous.

    </div>
    <div class="paragraph">

    The `ALL` qualifier resolves to true if the comparison is true for all of the values in the result of the subquery.
    It resolves to false if the subquery result is empty.

    </div>
    <div id="hql-all-subquery-comparison-qualifier-example" class="exampleblock">
    <div class="title">Example 396. ALL subquery comparison qualifier example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`// select all persons with all calls shorter than 50 seconds
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select distinct p.person " +
        "from Phone p " +
        "join p.calls c " +
        "where 50 &gt; all ( " +
        "    select duration" +
        "    from Call" +
        "    where phone = p " +
        ") ", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `ANY`/`SOME` qualifier resolves to true if the comparison is true for some of (at least one of) the values in the result of the subquery.
    It resolves to false if the subquery result is empty.

    </div>
    </div>
    <div class="sect2">