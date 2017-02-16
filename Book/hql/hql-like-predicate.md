 ### 15.42. Like predicate

    <div class="paragraph">

    Performs a like comparison on string values. The syntax is:

    </div>
    <div id="hql-like-predicate-bnf" class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`like_expression ::=
        string_expression
        [NOT] LIKE pattern_value
        [ESCAPE escape_character]`</pre>
    </div>
    </div>
    <div class="paragraph">

    The semantics follow that of the SQL like expression.
    The `pattern_value` is the pattern to attempt to match in the `string_expression`.
    Just like SQL, `pattern_value` can use `_` and `%` as wildcards.
    The meanings are the same. The `_` symbol matches any single character and `%` matches any number of characters.

    </div>
    <div id="hql-like-predicate-example" class="exampleblock">
    <div class="title">Example 398. Like predicate examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like 'Jo%'", Person.class )
    .getResultList();

    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name not like 'Jo%'", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The optional `escape 'escape character'` is used to specify an escape character used to escape the special meaning of `_` and `%` in the `pattern_value`.
    This is useful when needing to search on patterns including either `_` or `%`.

    </div>
    <div class="paragraph">

    The syntax is formed as follows: `'like_predicate' escape 'escape_symbol'`
    So, if `|` is the escape symbol and we want to match all stored procedures prefixed with `Dr_`, the like criteria becomes: `'Dr|_%' escape '|'`:

    </div>
    <div id="hql-like-predicate-escape-example" class="exampleblock">
    <div class="title">Example 399. Like with escape symbol</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`// find any person with a name starting with "Dr_"
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like 'Dr|_%' escape '|'", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">