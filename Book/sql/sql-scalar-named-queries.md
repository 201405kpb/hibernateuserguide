 #### 17.10.1. Named SQL queries selecting scalar values

    <div class="paragraph">

    To fetch a single column of given table, the named query looks as follows:

    </div>
    <div id="sql-scalar-NamedNativeQuery-example" class="exampleblock">
    <div class="title">Example 446. Single scalar value `NamedNativeQuery`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@NamedNativeQuery(
        name = "find_person_name",
        query =
            "SELECT name " +
            "FROM person "
    ),`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-jpa-scalar-named-query-example" class="exampleblock">
    <div class="title">Example 447. JPA named native query selecting a scalar value</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;String&gt; names = entityManager.createNamedQuery(
        "find_person_name" )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-scalar-named-query-example" class="exampleblock">
    <div class="title">Example 448. Hibernate named native query selecting a scalar value</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;String&gt; names = session.getNamedQuery(
        "find_person_name" )
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Selecting multiple scalar values is done like this:

    </div>
    <div id="sql-multiple-scalar-values-NamedNativeQuery-example" class="exampleblock">
    <div class="title">Example 449. Multiple scalar values `NamedNativeQuery`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@NamedNativeQuery(
        name = "find_person_name_and_nickName",
        query =
            "SELECT " +
            "   name, " +
            "   nickName " +
            "FROM person "
    ),`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Without specifying an explicit result type, Hibernate will assume an `Object` array:

    </div>
    <div id="sql-jpa-multiple-scalar-values-named-query-example" class="exampleblock">
    <div class="title">Example 450. JPA named native query selecting multiple scalar values</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; tuples = entityManager.createNamedQuery(
        "find_person_name_and_nickName" )
    .getResultList();

    for(Object[] tuple : tuples) {
        String name = (String) tuple[0];
        String nickName = (String) tuple[1];
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-multiple-scalar-values-named-query-example" class="exampleblock">
    <div class="title">Example 451. Hibernate named native query selecting multiple scalar values</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; tuples = session.getNamedQuery(
        "find_person_name_and_nickName" )
    .list();

    for(Object[] tuple : tuples) {
        String name = (String) tuple[0];
        String nickName = (String) tuple[1];
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    It&#8217;s possible to use a DTO to store the resulting scalar values:

    </div>
    <div id="sql-ConstructorResult-dto-example" class="exampleblock">
    <div class="title">Example 452. DTO to store multiple scalar values</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public class PersonNames {

        private final String name;

        private final String nickName;

        public PersonNames(String name, String nickName) {
            this.name = name;
            this.nickName = nickName;
        }

        public String getName() {
            return name;
        }

        public String getNickName() {
            return nickName;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-multiple-scalar-values-dto-NamedNativeQuery-example" class="exampleblock">
    <div class="title">Example 453. Multiple scalar values `NamedNativeQuery` with `ConstructorResult`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@NamedNativeQuery(
        name = "find_person_name_and_nickName_dto",
        query =
            "SELECT " +
            "   name, " +
            "   nickName " +
            "FROM person ",
        resultSetMapping = "name_and_nickName_dto"
    ),
    @SqlResultSetMapping(
        name = "name_and_nickName_dto",
        classes = @ConstructorResult(
            targetClass = PersonNames.class,
            columns = {
                @ColumnResult(name = "name"),
                @ColumnResult(name = "nickName")
            }
        )
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-jpa-multiple-scalar-values-dto-named-query-example" class="exampleblock">
    <div class="title">Example 454. JPA named native query selecting multiple scalar values into a DTO</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;PersonNames&gt; personNames = entityManager.createNamedQuery(
        "find_person_name_and_nickName_dto" )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-multiple-scalar-values-dto-named-query-example" class="exampleblock">
    <div class="title">Example 455. Hibernate named native query selecting multiple scalar values into a DTO</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;PersonNames&gt; personNames = session.getNamedQuery(
        "find_person_name_and_nickName_dto" )
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">