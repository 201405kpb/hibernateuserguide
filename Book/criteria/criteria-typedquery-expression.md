  ### 16.3. Selecting an expression

    <div class="paragraph">

    The simplest form of selecting an expression is selecting a particular attribute from an entity.
    But this expression might also represent an aggregation, a mathematical operation, etc.

    </div>
    <div id="criteria-typedquery-expression-example" class="exampleblock">
    <div class="title">Example 410. Selecting an attribute</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CriteriaBuilder builder = entityManager.getCriteriaBuilder();

    CriteriaQuery&lt;String&gt; criteria = builder.createQuery( String.class );
    Root&lt;Person&gt; root = criteria.from( Person.class );
    criteria.select( root.get( Person_.nickName ) );
    criteria.where( builder.equal( root.get( Person_.name ), "John Doe" ) );

    List&lt;String&gt; nickNames = entityManager.createQuery( criteria ).getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In this example, the query is typed as `java.lang.String` because that is the anticipated type of the results (the type of the `Person#nickName` attribute is `java.lang.String`).
    Because a query might contain multiple references to the `Person` entity, attribute references always need to be qualified.
    This is accomplished by the `Root#get` method call.

    </div>
    </div>
    <div class="sect2">