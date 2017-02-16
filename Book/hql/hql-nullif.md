 ### 15.36. NULLIF expressions

    <div class="paragraph">

    NULLIF is an abbreviated CASE expression that returns NULL if its operands are considered equal.

    </div>
    <div id="hql-nullif-example" class="exampleblock">
    <div class="title">Example 391. NULLIF example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;String&gt; nickNames = entityManager.createQuery(
        "select nullif( p.nickName, p.name ) " +
        "from Person p", String.class )
    .getResultList();

    // equivalent CASE expression
    List&lt;String&gt; nickNames = entityManager.createQuery(
        "select " +
        "    case" +
        "    when p.nickName = p.name" +
        "    then null" +
        "    else p.nickName" +
        "    end " +
        "from Person p", String.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">