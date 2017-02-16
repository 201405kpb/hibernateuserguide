### 16.6. Tuple criteria queries

    <div class="paragraph">

    A better approach to [Selecting multiple values](#criteria-typedquery-multiselect) is to use either a wrapper (which we just saw in [Selecting a wrapper](#criteria-typedquery-wrapper)) or using the `javax.persistence.Tuple` contract.

    </div>
    <div id="criteria-tuple-example" class="exampleblock">
    <div class="title">Example 414. Selecting a tuple</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CriteriaBuilder builder = entityManager.getCriteriaBuilder();

    CriteriaQuery&lt;Tuple&gt; criteria = builder.createQuery( Tuple.class );
    Root&lt;Person&gt; root = criteria.from( Person.class );

    Path&lt;Long&gt; idPath = root.get( Person_.id );
    Path&lt;String&gt; nickNamePath = root.get( Person_.nickName);

    criteria.multiselect( idPath, nickNamePath );
    criteria.where( builder.equal( root.get( Person_.name ), "John Doe" ) );

    List&lt;Tuple&gt; tuples = entityManager.createQuery( criteria ).getResultList();

    for ( Tuple tuple : tuples ) {
        Long id = tuple.get( idPath );
        String nickName = tuple.get( nickNamePath );
    }

    //or using indices
    for ( Tuple tuple : tuples ) {
        Long id = (Long) tuple.get( 0 );
        String nickName = (String) tuple.get( 1 );
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This example illustrates accessing the query results through the `javax.persistence.Tuple` interface.
    The example uses the explicit `createTupleQuery()` of `javax.persistence.criteria.CriteriaBuilder`.
    An alternate approach is to use `createQuery( Tuple.class )`.

    </div>
    <div class="paragraph">

    Again we see the use of the `multiselect()` method, just like in [Selecting an array using `multiselect`](#criteria-typedquery-multiselect-array-implicit-example).
    The difference here is that the type of the `javax.persistence.criteria.CriteriaQuery` was defined as `javax.persistence.Tuple` so the compound selections, in this case, are interpreted to be the tuple elements.

    </div>
    <div class="paragraph">

    The javax.persistence.Tuple contract provides three forms of access to the underlying elements:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">typed</dt>
    <dd>

    The [Selecting a tuple](#criteria-tuple-example) example illustrates this form of access in the `tuple.get( idPath )` and `tuple.get( nickNamePath )` calls.
    This allows typed access to the underlying tuple values based on the `javax.persistence.TupleElement` expressions used to build the criteria.

    </dd>
    <dt class="hdlist1">positional</dt>
    <dd>

    Allows access to the underlying tuple values based on the position.
    The simple _Object get(int position)_ form is very similar to the access illustrated in [Selecting an array](#criteria-typedquery-multiselect-array-explicit-example) and [Selecting an array using `multiselect`](#criteria-typedquery-multiselect-array-implicit-example).
    The _&lt;X&gt; X get(int position, Class&lt;X&gt; type_ form allows typed positional access, but based on the explicitly supplied type which the tuple value must be type-assignable to.

    </dd>
    <dt class="hdlist1">aliased</dt>
    <dd>

    Allows access to the underlying tuple values based an (optionally) assigned alias.
    The example query did not apply an alias.
    An alias would be applied via the alias method on `javax.persistence.criteria.Selection`.
    Just like `positional` access, there is both a typed (_Object get(String alias)_) and an untyped (_&lt;X&gt; X get(String alias, Class&lt;X&gt; type_ form.

    </dd>
    </dl>
    </div>
    </div>
    <div class="sect2">