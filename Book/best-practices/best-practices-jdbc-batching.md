 ### 25.3. JDBC batching

    <div class="paragraph">

    JDBC allows us to batch multiple SQL statements and to send them to the database server into a single request.
    This saves database roundtrips, and so it [reduces response time significantly](https://leanpub.com/high-performance-java-persistence/read#jdbc-batch-updates).

    </div>
    <div class="paragraph">

    Not only `INSERT` and `UPDATE` statements, but even `DELETE` statements can be batched as well.
    For `INSERT` and `UPDATE` statements, make sure that you have all the right configuration properties in place, like ordering inserts and updates and activating batching for versioned data.
    Check out this article for more details on this topic.

    </div>
    <div class="paragraph">

    For `DELETE` statements, there is no option to order parent and child statements, so cascading can interfere with the JDBC batching process.

    </div>
    <div class="paragraph">

    Unlike any other framework which doesn&#8217;t automate SQL statement generation, Hibernate makes it very easy to activate JDBC-level batching as indicated in the [Batching chapter](#batch).

    </div>
    </div>
    <div class="sect2">
