 ### 15.47. Member-of collection predicate

    <div class="paragraph">

    The `[NOT] MEMBER [OF]` expression applies to collection-valued path expressions.
    It checks whether a value is a member of the specified collection.

    </div>
    <div id="hql-member-of-collection-predicate-example" class="exampleblock">
    <div class="title">Example 403. Member-of collection expression examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where 'Home address' member of p.addresses", Person.class )
    .getResultList();

    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where 'Home address' not member of p.addresses", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">