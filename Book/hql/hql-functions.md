### 15.29. HQL functions

    <div class="paragraph">

    Beyond the JPQL standardized functions, HQL makes some additional functions available regardless of the underlying database in use.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">BIT_LENGTH</dt>
    <dd>

    Returns the length of binary data.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Number&gt; bits = entityManager.createQuery(
        "select bit_length( c.duration ) " +
        "from Call c ", Number.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">CAST</dt>
    <dd>

    Performs a SQL cast.
    The cast target should name the Hibernate mapping type to use.
    See the [data types](#basic-provided) chapter on for more information.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;String&gt; durations = entityManager.createQuery(
        "select cast( c.duration as string ) " +
        "from Call c ", String.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">EXTRACT</dt>
    <dd>

    Performs a SQL extraction on datetime values.
    An extraction extracts parts of the datetime (the year, for example).

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Integer&gt; years = entityManager.createQuery(
        "select extract( YEAR from c.timestamp ) " +
        "from Call c ", Integer.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    See the abbreviated forms below.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">YEAR</dt>
    <dd>

    Abbreviated extract form for extracting the year.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Integer&gt; years = entityManager.createQuery(
        "select year( c.timestamp ) " +
        "from Call c ", Integer.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">MONTH</dt>
    <dd>

    Abbreviated extract form for extracting the month.

    </dd>
    <dt class="hdlist1">DAY</dt>
    <dd>

    Abbreviated extract form for extracting the day.

    </dd>
    <dt class="hdlist1">HOUR</dt>
    <dd>

    Abbreviated extract form for extracting the hour.

    </dd>
    <dt class="hdlist1">MINUTE</dt>
    <dd>

    Abbreviated extract form for extracting the minute.

    </dd>
    <dt class="hdlist1">SECOND</dt>
    <dd>

    Abbreviated extract form for extracting the second.

    </dd>
    <dt class="hdlist1">STR</dt>
    <dd>

    Abbreviated form for casting a value as character data.

    </dd>
    </dl>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;String&gt; timestamps = entityManager.createQuery(
        "select str( c.timestamp ) " +
        "from Call c ", String.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">
