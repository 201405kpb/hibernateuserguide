 ### 16.8. Roots

    <div class="paragraph">

    Roots define the basis from which all joins, paths and attributes are available in the query.
    A root is always an entity type. Roots are defined and added to the criteria by the overloaded from methods on `javax.persistence.criteria.CriteriaQuery`:

    </div>
    <div id="criteria-from-root-methods-example" class="exampleblock">
    <div class="title">Example 415. Root methods</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;X&gt; Root&lt;X&gt; from( Class&lt;X&gt; );

    &lt;X&gt; Root&lt;X&gt; from( EntityType&lt;X&gt; );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="criteria-from-root-example" class="exampleblock">
    <div class="title">Example 416. Adding a root example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CriteriaBuilder builder = entityManager.getCriteriaBuilder();

    CriteriaQuery&lt;Person&gt; criteria = builder.createQuery( Person.class );
    Root&lt;Person&gt; root = criteria.from( Person.class );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Criteria queries may define multiple roots, the effect of which is to create a Cartesian Product between the newly added root and the others.
    Here is an example defining a Cartesian Product between `Person` and `Partner` entities:

    </div>
    <div id="criteria-from-multiple-root-example" class="exampleblock">
    <div class="title">Example 417. Adding multiple roots example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CriteriaBuilder builder = entityManager.getCriteriaBuilder();

    CriteriaQuery&lt;Tuple&gt; criteria = builder.createQuery( Tuple.class );

    Root&lt;Person&gt; personRoot = criteria.from( Person.class );
    Root&lt;Partner&gt; partnerRoot = criteria.from( Partner.class );
    criteria.multiselect( personRoot, partnerRoot );

    Predicate personRestriction = builder.and(
        builder.equal( personRoot.get( Person_.address ), address ),
        builder.isNotEmpty( personRoot.get( Person_.phones ) )
    );
    Predicate partnerRestriction = builder.and(
        builder.like( partnerRoot.get( Partner_.name ), prefix ),
        builder.equal( partnerRoot.get( Partner_.version ), 0 )
    );
    criteria.where( builder.and( personRestriction, partnerRestriction ) );

    List&lt;Tuple&gt; tuples = entityManager.createQuery( criteria ).getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">