 ### 15.23. Literals

    <div class="paragraph">

    String literals are enclosed in single quotes.
    To escape a single quote within a string literal, use double single quotes.

    </div>
    <div id="hql-string-literals-example" class="exampleblock">
    <div class="title">Example 381. String literals examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like 'Joe'", Person.class)
    .getResultList();

    // Escaping quotes
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like 'Joe''s'", Person.class)
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Numeric literals are allowed in a few different forms.

    </div>
    <div id="hql-numeric-literals-example" class="exampleblock">
    <div class="title">Example 382. Numeric literal examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`// simple integer literal
    Person person = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.id = 1", Person.class)
    .getSingleResult();

    // simple integer literal, typed as a long
    Person person = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.id = 1L", Person.class)
    .getSingleResult();

    // decimal notation
    List&lt;Call&gt; calls = entityManager.createQuery(
        "select c " +
        "from Call c " +
        "where c.duration &gt; 100.5", Call.class )
    .getResultList();

    // decimal notation, typed as a float
    List&lt;Call&gt; calls = entityManager.createQuery(
        "select c " +
        "from Call c " +
        "where c.duration &gt; 100.5F", Call.class )
    .getResultList();

    // scientific notation
    List&lt;Call&gt; calls = entityManager.createQuery(
        "select c " +
        "from Call c " +
        "where c.duration &gt; 1e+2", Call.class )
    .getResultList();

    // scientific notation, typed as a float
    List&lt;Call&gt; calls = entityManager.createQuery(
        "select c " +
        "from Call c " +
        "where c.duration &gt; 1e+2F", Call.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    In the scientific notation form, the `E` is case-insensitive.

    </div>
    <div class="paragraph">

    Specific typing can be achieved through the use of the same suffix approach specified by Java.
    So, `L` denotes a long, `D` denotes a double, `F` denotes a float.
    The actual suffix is case-insensitive.

    </div>
    <div class="paragraph">

    The boolean literals are `TRUE` and `FALSE`, again case-insensitive.

    </div>
    <div class="paragraph">

    Enums can even be referenced as literals. The fully-qualified enum class name must be used.
    HQL can also handle constants in the same manner, though JPQL does not define that as supported.

    </div>
    <div class="paragraph">

    Entity names can also be used as literal. See [Entity type](#hql-entity-type-exp).

    </div>
    <div class="paragraph">

    Date/time literals can be specified using the JDBC escape syntax:

    </div>
    <div class="ulist">

*   `{d 'yyyy-mm-dd'}` for dates
*   `{t 'hh:mm:ss'}` for times
*   `{ts 'yyyy-mm-dd hh:mm:ss[.millis]'}` (millis optional) for timestamps.
    </div>
    <div class="paragraph">

    These Date/time literals only work if you JDBC drivers supports them.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">