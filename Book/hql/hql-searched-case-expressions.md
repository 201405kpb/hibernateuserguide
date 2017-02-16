 ### 15.35. Searched CASE expressions

    <div class="paragraph">

    The searched form has the following syntax:

    </div>
    <div id="hql-searched-case-expressions-bnf" class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CASE [ WHEN {test_conditional} THEN {match_result} ]* ELSE {miss_result} END`</pre>
    </div>
    </div>
    <div id="hql-searched-case-expressions-example" class="exampleblock">
    <div class="title">Example 390. Searched case expression example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;String&gt; nickNames = entityManager.createQuery(
        "select " +
        "    case " +
        "    when p.nickName is null " +
        "    then " +
        "        case " +
        "        when p.name is null " +
        "        then '&lt;no nick name&gt;' " +
        "        else p.name " +
        "        end" +
        "    else p.nickName " +
        "    end " +
        "from Person p", String.class )
    .getResultList();

    // coalesce can handle this more succinctly
    List&lt;String&gt; nickNames = entityManager.createQuery(
        "select coalesce( p.nickName, p.name, '&lt;no nick name&gt;' ) " +
        "from Person p", String.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">