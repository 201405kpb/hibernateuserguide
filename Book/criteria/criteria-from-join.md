### 16.9. Joins

    <div class="paragraph">

    Joins allow navigation from other `javax.persistence.criteria.From` to either association or embedded attributes.
    Joins are created by the numerous overloaded join methods of the `javax.persistence.criteria.From` interface.

    </div>
    <div id="criteria-from-join-example" class="exampleblock">
    <div class="title">Example 418. Join example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CriteriaBuilder builder = entityManager.getCriteriaBuilder();

    CriteriaQuery&lt;Phone&gt; criteria = builder.createQuery( Phone.class );
    Root&lt;Phone&gt; root = criteria.from( Phone.class );

    // Phone.person is a @ManyToOne
    Join&lt;Phone, Person&gt; personJoin = root.join( Phone_.person );
    // Person.addresses is an @ElementCollection
    Join&lt;Person, String&gt; addressesJoin = personJoin.join( Person_.addresses );

    criteria.where( builder.isNotEmpty( root.get( Phone_.calls ) ) );

    List&lt;Phone&gt; phones = entityManager.createQuery( criteria ).getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">