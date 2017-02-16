 ### 15.28. JPQL standardized functions

    <div class="paragraph">

    Here is the list of functions defined as supported by JPQL.
    Applications interested in remaining portable between JPA providers should stick to these functions.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">CONCAT</dt>
    <dd>

    String concatenation function. Variable argument length of 2 or more string values to be concatenated together.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;String&gt; callHistory = entityManager.createQuery(
        "select concat( p.number, ' : ' ,c.duration ) " +
        "from Call c " +
        "join c.phone p", String.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">SUBSTRING</dt>
    <dd>

    Extracts a portion of a string value.
    The second argument denotes the starting position. The third (optional) argument denotes the length.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;String&gt; prefixes = entityManager.createQuery(
        "select substring( p.number, 0, 2 ) " +
        "from Call c " +
        "join c.phone p", String.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">UPPER</dt>
    <dd>

    Upper cases the specified string

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;String&gt; names = entityManager.createQuery(
        "select upper( p.name ) " +
        "from Person p ", String.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">LOWER</dt>
    <dd>

    Lower cases the specified string

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;String&gt; names = entityManager.createQuery(
        "select lower( p.name ) " +
        "from Person p ", String.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">TRIM</dt>
    <dd>

    Follows the semantics of the SQL trim function.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;String&gt; names = entityManager.createQuery(
        "select trim( p.name ) " +
        "from Person p ", String.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">LENGTH</dt>
    <dd>

    Returns the length of a string.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Integer&gt; lengths = entityManager.createQuery(
        "select length( p.name ) " +
        "from Person p ", Integer.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">LOCATE</dt>
    <dd>

    Locates a string within another string.
    The third argument (optional) is used to denote a position from which to start looking.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Integer&gt; sizes = entityManager.createQuery(
        "select locate( 'John', p.name ) " +
        "from Person p ", Integer.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">ABS</dt>
    <dd>

    Calculates the mathematical absolute value of a numeric value.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Integer&gt; abs = entityManager.createQuery(
        "select abs( c.duration ) " +
        "from Call c ", Integer.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">MOD</dt>
    <dd>

    Calculates the remainder of dividing the first argument by the second.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Integer&gt; mods = entityManager.createQuery(
        "select mod( c.duration, 10 ) " +
        "from Call c ", Integer.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">SQRT</dt>
    <dd>

    Calculates the mathematical square root of a numeric value.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Double&gt; sqrts = entityManager.createQuery(
        "select sqrt( c.duration ) " +
        "from Call c ", Double.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">CURRENT_DATE</dt>
    <dd>

    Returns the database current date.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Call&gt; calls = entityManager.createQuery(
        "select c " +
        "from Call c " +
        "where c.timestamp = current_date", Call.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">CURRENT_TIME</dt>
    <dd>

    Returns the database current time.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Call&gt; calls = entityManager.createQuery(
        "select c " +
        "from Call c " +
        "where c.timestamp = current_time", Call.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">CURRENT_TIMESTAMP</dt>
    <dd>

    Returns the database current timestamp.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Call&gt; calls = entityManager.createQuery(
        "select c " +
        "from Call c " +
        "where c.timestamp = current_timestamp", Call.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">