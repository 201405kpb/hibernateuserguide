#### 2.8.5. Ordered Lists

    <div class="paragraph">

    Although they use the `List` interface on the Java side, bags don&#8217;t retain element order.
    To preserve the collection element order, there are two possibilities:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`@OrderBy`</dt>
    <dd>

    the collection is ordered upon retrieval using a child entity property

    </dd>
    <dt class="hdlist1">`@OrderColumn`</dt>
    <dd>

    the collection uses a dedicated order column in the collection link table

    </dd>
    </dl>
    </div>
    <div class="sect4">

    ##### Unidirectional ordered lists

    <div class="paragraph">

    When using the `@OrderBy` annotation, the mapping looks as follows:

    </div>
    <div id="collections-unidirectional-ordered-list-order-by-example" class="exampleblock">
    <div class="title">Example 152. Unidirectional `@OrderBy` list</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;
        @OneToMany(cascade = CascadeType.ALL)
        @OrderBy("number")
        private List&lt;Phone&gt; phones = new ArrayList&lt;&gt;();

        public Person() {
        }

        public Person(Long id) {
            this.id = id;
        }

        public List&lt;Phone&gt; getPhones() {
            return phones;
        }
    }

    @Entity(name = "Phone")
    public static class Phone {

        @Id
        private Long id;

        private String type;

        @Column(name = "`number`")
        private String number;

        public Phone() {
        }

        public Phone(Long id, String type, String number) {
            this.id = id;
            this.type = type;
            this.number = number;
        }

        public Long getId() {
            return id;
        }

        public String getType() {
            return type;
        }

        public String getNumber() {
            return number;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The database mapping is the same as with the [Unidirectional bags](#collections-unidirectional-bag) example, so it won&#8217;t be repeated.
    Upon fetching the collection, Hibernate generates the following select statement:

    </div>
    <div id="collections-unidirectional-ordered-list-order-by-select-example" class="exampleblock">
    <div class="title">Example 153. Unidirectional `@OrderBy` list select statement</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT
       phones0_.Person_id AS Person_i1_1_0_,
       phones0_.phones_id AS phones_i2_1_0_,
       unidirecti1_.id AS id1_2_1_,
       unidirecti1_."number" AS number2_2_1_,
       unidirecti1_.type AS type3_2_1_
    FROM
       Person_Phone phones0_
    INNER JOIN
       Phone unidirecti1_ ON phones0_.phones_id=unidirecti1_.id
    WHERE
       phones0_.Person_id = 1
    ORDER BY
       unidirecti1_."number"`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The child table column is used to order the list elements.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The `@OrderBy` annotation can take multiple entity properties, and each property can take an ordering direction too (e.g. `@OrderBy("name ASC, type DESC")`).

    </div>
    <div class="paragraph">

    If no property is specified (e.g. `@OrderBy`), the primary key of the child entity table is used for ordering.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Another ordering option is to use the `@OrderColumn` annotation:

    </div>
    <div id="collections-unidirectional-ordered-list-order-column-example" class="exampleblock">
    <div class="title">Example 154. Unidirectional `@OrderColumn` list</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@OneToMany(cascade = CascadeType.ALL)
    @OrderColumn(name = "order_id")
    private List&lt;Phone&gt; phones = new ArrayList&lt;&gt;();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Person_Phone (
        Person_id BIGINT NOT NULL ,
        phones_id BIGINT NOT NULL ,
        order_id INTEGER NOT NULL ,
        PRIMARY KEY ( Person_id, order_id )
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This time, the link table takes the `order_id` column and uses it to materialize the collection element order.
    When fetching the list, the following select query is executed:

    </div>
    <div id="collections-unidirectional-ordered-list-order-column-select-example" class="exampleblock">
    <div class="title">Example 155. Unidirectional `@OrderColumn` list select statement</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`select
       phones0_.Person_id as Person_i1_1_0_,
       phones0_.phones_id as phones_i2_1_0_,
       phones0_.order_id as order_id3_0_,
       unidirecti1_.id as id1_2_1_,
       unidirecti1_.number as number2_2_1_,
       unidirecti1_.type as type3_2_1_
    from
       Person_Phone phones0_
    inner join
       Phone unidirecti1_
          on phones0_.phones_id=unidirecti1_.id
    where
       phones0_.Person_id = 1`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    With the `order_id` column in place, Hibernate can order the list in-memory after it&#8217;s being fetched from the database.

    </div>
    </div>
    <div class="sect4">

    ##### Bidirectional ordered lists

    <div class="paragraph">

    The mapping is similar with the [Bidirectional bags](#collections-bidirectional-bag) example, just that the parent side is going to be annotated with either `@OrderBy` or `@OrderColumn`.

    </div>
    <div id="collections-bidirectional-ordered-list-order-by-example" class="exampleblock">
    <div class="title">Example 156. Bidirectional `@OrderBy` list</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@OneToMany(mappedBy = "person", cascade = CascadeType.ALL)
    @OrderBy("number")
    private List&lt;Phone&gt; phones = new ArrayList&lt;&gt;();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Just like with the unidirectional `@OrderBy` list, the `number` column is used to order the statement on the SQL level.

    </div>
    <div class="paragraph">

    When using the `@OrderColumn` annotation, the `order_id` column is going to be embedded in the child table:

    </div>
    <div id="collections-bidirectional-ordered-list-order-column-example" class="exampleblock">
    <div class="title">Example 157. Bidirectional `@OrderColumn` list</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@OneToMany(mappedBy = "person", cascade = CascadeType.ALL)
    @OrderColumn(name = "order_id")
    private List&lt;Phone&gt; phones = new ArrayList&lt;&gt;();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Phone (
        id BIGINT NOT NULL ,
        number VARCHAR(255) ,
        type VARCHAR(255) ,
        person_id BIGINT ,
        order_id INTEGER ,
        PRIMARY KEY ( id )
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When fetching the collection, Hibernate will use the fetched ordered columns to sort the elements according to the `@OrderColumn` mapping.

    </div>
    </div>
    </div>
    <div class="sect3">