### 16.12. Using parameters

    <div id="criteria-param-example" class="exampleblock">
    <div class="title">Example 420. Parameters example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CriteriaBuilder builder = entityManager.getCriteriaBuilder();

    CriteriaQuery&lt;Person&gt; criteria = builder.createQuery( Person.class );
    Root&lt;Person&gt; root = criteria.from( Person.class );

    ParameterExpression&lt;String&gt; nickNameParameter = builder.parameter( String.class );
    criteria.where( builder.equal( root.get( Person_.nickName ), nickNameParameter ) );

    TypedQuery&lt;Person&gt; query = entityManager.createQuery( criteria );
    query.setParameter( nickNameParameter, "JD" );
    List&lt;Person&gt; persons = query.getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Use the parameter method of `javax.persistence.criteria.CriteriaBuilder` to obtain a parameter reference.
    Then use the parameter reference to bind the parameter value to the `javax.persistence.Query`.

    </div>
    </div>
    <div class="sect2">