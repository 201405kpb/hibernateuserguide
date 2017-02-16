 ### 16.4. Selecting multiple values

    <div class="paragraph">

    There are actually a few different ways to select multiple values using criteria queries.
    We will explore two options here, but an alternative recommended approach is to use tuples as described in [Tuple criteria queries](#criteria-tuple),
    or consider a wrapper query, see [Selecting a wrapper](#criteria-typedquery-wrapper) for details.

    </div>
    <div id="criteria-typedquery-multiselect-array-explicit-example" class="exampleblock">
    <div class="title">Example 411. Selecting an array</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CriteriaBuilder builder = entityManager.getCriteriaBuilder();

    CriteriaQuery&lt;Object[]&gt; criteria = builder.createQuery( Object[].class );
    Root&lt;Person&gt; root = criteria.from( Person.class );

    Path&lt;Long&gt; idPath = root.get( Person_.id );
    Path&lt;String&gt; nickNamePath = root.get( Person_.nickName);

    criteria.select( builder.array( idPath, nickNamePath ) );
    criteria.where( builder.equal( root.get( Person_.name ), "John Doe" ) );

    List&lt;Object[]&gt; idAndNickNames = entityManager.createQuery( criteria ).getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Technically this is classified as a typed query, but you can see from handling the results that this is sort of misleading.
    Anyway, the expected result type here is an array.

    </div>
    <div class="paragraph">

    The example then uses the array method of `javax.persistence.criteria.CriteriaBuilder` which explicitly combines individual selections into a `javax.persistence.criteria.CompoundSelection`.

    </div>
    <div id="criteria-typedquery-multiselect-array-implicit-example" class="exampleblock">
    <div class="title">Example 412. Selecting an array using `multiselect`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CriteriaBuilder builder = entityManager.getCriteriaBuilder();

    CriteriaQuery&lt;Object[]&gt; criteria = builder.createQuery( Object[].class );
    Root&lt;Person&gt; root = criteria.from( Person.class );

    Path&lt;Long&gt; idPath = root.get( Person_.id );
    Path&lt;String&gt; nickNamePath = root.get( Person_.nickName);

    criteria.multiselect( idPath, nickNamePath );
    criteria.where( builder.equal( root.get( Person_.name ), "John Doe" ) );

    List&lt;Object[]&gt; idAndNickNames = entityManager.createQuery( criteria ).getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Just as we saw in [Selecting an array](#criteria-typedquery-multiselect-array-explicit-example) we have a typed criteria query returning an `Object` array.
    Both queries are functionally equivalent.
    This second example uses the `multiselect()` method which behaves slightly differently based on the type given when the criteria query was first built,
    but, in this case, it says to select and return an _Object[]_.

    </div>
    </div>
    <div class="sect2">
