  ### 15.34. Simple CASE expressions

    <div class="paragraph">

    The simple form has the following syntax:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CASE {operand} WHEN {test_value} THEN {match_result} ELSE {miss_result} END`</pre>
    </div>
    </div>
    <div id="hql-simple-case-expressions-example" class="exampleblock">
    <div class="title">Example 389. Simple case expression example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;String&gt; nickNames = entityManager.createQuery(
        "select " +
        "    case p.nickName " +
        "    when 'NA' " +
        "    then '&lt;no nick name&gt;' " +
        "    else p.nickName " +
        "    end " +
        "from Person p", String.class )
    .getResultList();

    // same as above
    List&lt;String&gt; nickNames = entityManager.createQuery(
        "select coalesce(p.nickName, '&lt;no nick name&gt;') " +
        "from Person p", String.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">