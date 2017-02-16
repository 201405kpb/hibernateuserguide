### 15.38. The `SELECT` clause

    <div class="paragraph">

    The `SELECT` clause identifies which objects and values to return as the query results.
    The expressions discussed in [Expressions](#hql-expressions) are all valid select expressions, except where otherwise noted.
    See the section [Hibernate Query API](#hql-api) for information on handling the results depending on the types of values specified in the `SELECT` clause.

    </div>
    <div class="paragraph">

    There is a particular expression type that is only valid in the select clause.
    Hibernate calls this "dynamic instantiation".
    JPQL supports some of that feature and calls it a "constructor expression".

    </div>
    <div class="paragraph">

    So rather than dealing with the `Object[]` (again, see [Hibernate Query API](#hql-api)) here we are wrapping the values in a type-safe java object that will be returned as the results of the query.

    </div>
    <div id="hql-select-clause-dynamic-instantiation-example" class="exampleblock">
    <div class="title">Example 392. Dynamic HQL and JPQL instantiation example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public class CallStatistics {

        private final long count;
        private final long total;
        private final int min;
        private final int max;
        private final double abg;

        public CallStatistics(long count, long total, int min, int max, double abg) {
            this.count = count;
            this.total = total;
            this.min = min;
            this.max = max;
            this.abg = abg;
        }

        //Getters and setters omitted for brevity
    }

    CallStatistics callStatistics = entityManager.createQuery(
        "select new org.hibernate.userguide.hql.CallStatistics(" +
        "    count(c), " +
        "    sum(c.duration), " +
        "    min(c.duration), " +
        "    max(c.duration), " +
        "    avg(c.duration)" +
        ")  " +
        "from Call c ", CallStatistics.class )
    .getSingleResult();`</pre>
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

    The class reference must be fully qualified and it must have a matching constructor.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The class here need not be mapped.
    If it does represent an entity, the resulting instances are returned in the NEW state (not managed!).

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    HQL supports additional "dynamic instantiation" features.
    First, the query can specify to return a `List` rather than an `Object[]` for scalar results:

    </div>
    <div id="hql-select-clause-dynamic-list-instantiation-example" class="exampleblock">
    <div class="title">Example 393. Dynamic instantiation example - list</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;List&gt; phoneCallDurations = entityManager.createQuery(
        "select new list(" +
        "    p.number, " +
        "    c.duration " +
        ")  " +
        "from Call c " +
        "join c.phone p ", List.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The results from this query will be a `List&lt;List&gt;` as opposed to a `List&lt;Object[]&gt;`

    </div>
    <div class="paragraph">

    HQL also supports wrapping the scalar results in a `Map`.

    </div>
    <div id="hql-select-clause-dynamic-map-instantiation-example" class="exampleblock">
    <div class="title">Example 394. Dynamic instantiation example - map</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Map&gt; phoneCallTotalDurations = entityManager.createQuery(
        "select new map(" +
        "    p.number as phoneNumber , " +
        "    sum(c.duration) as totalDuration, " +
        "    avg(c.duration) as averageDuration " +
        ")  " +
        "from Call c " +
        "join c.phone p " +
        "group by p.number ", Map.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The results from this query will be a `List&lt;Map&lt;String, Object&gt;&gt;` as opposed to a `List&lt;Object[]&gt;`.
    The keys of the map are defined by the aliases given to the select expressions.
    If the user doesn&#8217;t assign aliases, the key will be the index of each particular result set column (e.g. 0, 1, 2, etc).

    </div>
    </div>
    <div class="sect2">