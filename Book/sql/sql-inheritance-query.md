### 17.8. Handling inheritance

    <div class="paragraph">

    Native SQL queries which query for entities that are mapped as part of an inheritance must include all properties for the base class and all its subclasses.

    </div>
    <div id="sql-hibernate-inheritance-query-example" class="exampleblock">
    <div class="title">Example 443. Hibernate native query selecting subclasses</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;CreditCardPayment&gt; payments = session.createSQLQuery(
        "SELECT * " +
        "FROM Payment p " +
        "JOIN CreditCardPayment cp on cp.id = p.id" )
    .addEntity( CreditCardPayment.class )
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

    There&#8217;s no such equivalent in JPA because the `Query` interface doesn&#8217;t define an `addEntity` method equivalent.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">