 ### 17.7. Returning DTOs (Data Transfer Objects)

    <div class="paragraph">

    It is possible to apply a `ResultTransformer` to native SQL queries, allowing it to return non-managed entities.

    </div>
    <div id="sql-hibernate-dto-query-example" class="exampleblock">
    <div class="title">Example 442. Hibernate native query selecting DTOs</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public class PersonSummaryDTO {

        private Number id;

        private String name;

        public Number getId() {
            return id;
        }

        public void setId(Number id) {
            this.id = id;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }

    List&lt;PersonSummaryDTO&gt; dtos = session.createSQLQuery(
        "SELECT p.id as \"id\", p.name as \"name\" " +
        "FROM person p")
    .setResultTransformer( Transformers.aliasToBean( PersonSummaryDTO.class ) )
    .list();`</pre>
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

    There&#8217;s no such equivalent in JPA because the `Query` interface doesn&#8217;t define a `setResultTransformer` method equivalent.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The above query will return a list of `PersonSummaryDTO` which has been instantiated and injected the values of `id` and `name` into its corresponding properties or fields.

    </div>
    </div>
    <div class="sect2">