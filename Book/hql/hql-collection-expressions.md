 ### 15.31. Collection-related expressions

    <div class="paragraph">

    There are a few specialized expressions for working with collection-valued associations.
    Generally, these are just abbreviated forms or other expressions for the sake of conciseness.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">SIZE</dt>
    <dd>

    Calculate the size of a collection. Equates to a subquery!

    </dd>
    <dt class="hdlist1">MAXELEMENT</dt>
    <dd>

    Available for use on collections of basic type.
    Refers to the maximum value as determined by applying the `max` SQL aggregation.

    </dd>
    <dt class="hdlist1">MAXINDEX</dt>
    <dd>

    Available for use on indexed collections.
    Refers to the maximum index (key/position) as determined by applying the `max` SQL aggregation.

    </dd>
    <dt class="hdlist1">MINELEMENT</dt>
    <dd>

    Available for use on collections of basic type.
    Refers to the minimum value as determined by applying the `min` SQL aggregation.

    </dd>
    <dt class="hdlist1">MININDEX</dt>
    <dd>

    Available for use on indexed collections.
    Refers to the minimum index (key/position) as determined by applying the `min` SQL aggregation.

    </dd>
    <dt class="hdlist1">ELEMENTS</dt>
    <dd>

    Used to refer to the elements of a collection as a whole.
    Only allowed in the where clause.
    Often used in conjunction with `ALL`, `ANY` or `SOME` restrictions.

    </dd>
    <dt class="hdlist1">INDICES</dt>
    <dd>

    Similar to `elements` except that `indices` refers to the collections indices (keys/positions) as a whole.

    </dd>
    </dl>
    </div>
    <div id="hql-collection-expressions-example" class="exampleblock">
    <div class="title">Example 386. Collection-related expressions examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Phone&gt; phones = entityManager.createQuery(
        "select p " +
        "from Phone p " +
        "where maxelement( p.calls ) = :call", Phone.class )
    .setParameter( "call", call )
    .getResultList();

    List&lt;Phone&gt; phones = entityManager.createQuery(
        "select p " +
        "from Phone p " +
        "where minelement( p.calls ) = :call", Phone.class )
    .setParameter( "call", call )
    .getResultList();

    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where maxindex( p.phones ) = 0", Person.class )
    .getResultList();

    // the above query can be re-written with member of
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where :phone member of p.phones", Person.class )
    .setParameter( "phone", phone )
    .getResultList();

    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where :phone = some elements ( p.phones )", Person.class )
    .setParameter( "phone", phone )
    .getResultList();

    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where exists elements ( p.phones )", Person.class )
    .getResultList();

    List&lt;Phone&gt; phones = entityManager.createQuery(
        "select p " +
        "from Phone p " +
        "where current_date() &gt; key( p.callHistory )", Phone.class )
    .getResultList();

    List&lt;Phone&gt; phones = entityManager.createQuery(
        "select p " +
        "from Phone p " +
        "where current_date() &gt; all elements( p.repairTimestamps )", Phone.class )
    .getResultList();

    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where 1 in indices( p.phones )", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Elements of indexed collections (arrays, lists, and maps) can be referred to by index operator.

    </div>
    <div id="hql-collection-index-operator-example" class="exampleblock">
    <div class="title">Example 387. Index operator examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`// indexed lists
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.phones[ 0 ].type = 'LAND_LINE'", Person.class )
    .getResultList();

    // maps
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.addresses[ 'HOME' ] = :address", Person.class )
    .setParameter( "address", address)
    .getResultList();

    //max index in list
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select pr " +
        "from Person pr " +
        "where pr.phones[ maxindex(pr.phones) ].type = 'LAND_LINE'", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    See also [Special case - qualified path expressions](#hql-collection-qualification) as there is a good deal of overlap.

    </div>
    </div>
    <div class="sect2">