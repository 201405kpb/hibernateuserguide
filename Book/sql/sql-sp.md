### 17.12. Using stored procedures for querying

    <div class="paragraph">

    Hibernate provides support for queries via stored procedures and functions.
    A stored procedure arguments are declared using the `IN` parameter type, and the result can be either marked with an `OUT`
    parameter type, a `REF_CURSOR` or it could just return the result like a function.

    </div>
    <div id="sql-sp-out-mysql-example" class="exampleblock">
    <div class="title">Example 466. MySQL stored procedure with `OUT` parameter type</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`statement.executeUpdate(
        "CREATE PROCEDURE sp_count_phones (" +
        "   IN personId INT, " +
        "   OUT phoneCount INT " +
        ") " +
        "BEGIN " +
        "    SELECT COUNT(*) INTO phoneCount " +
        "    FROM phone  " +
        "    WHERE phone.person_id = personId; " +
        "END"
    );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To use this stored procedure, you can execute the following JPA 2.1 query:

    </div>
    <div id="sql-jpa-call-sp-out-mysql-example" class="exampleblock">
    <div class="title">Example 467. Calling a MySQL stored procedure with `OUT` parameter type using JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`StoredProcedureQuery query = entityManager.createStoredProcedureQuery( "sp_count_phones");
    query.registerStoredProcedureParameter( "personId", Long.class, ParameterMode.IN);
    query.registerStoredProcedureParameter( "phoneCount", Long.class, ParameterMode.OUT);

    query.setParameter("personId", 1L);

    query.execute();
    Long phoneCount = (Long) query.getOutputParameterValue("phoneCount");`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-call-sp-out-mysql-example" class="exampleblock">
    <div class="title">Example 468. Calling a MySQL stored procedure with `OUT` parameter type using Hibernate</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session = entityManager.unwrap( Session.class );

    ProcedureCall call = session.createStoredProcedureCall( "sp_count_phones" );
    call.registerParameter( "personId", Long.class, ParameterMode.IN ).bindValue( 1L );
    call.registerParameter( "phoneCount", Long.class, ParameterMode.OUT );

    Long phoneCount = (Long) call.getOutputs().getOutputParameterValue( "phoneCount" );
    assertEquals( Long.valueOf( 2 ), phoneCount );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    If the stored procedure outputs the result directly without an `OUT` parameter type:

    </div>
    <div id="sql-sp-mysql-return-no-out-example" class="exampleblock">
    <div class="title">Example 469. MySQL stored procedure without an `OUT` parameter type</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`statement.executeUpdate(
        "CREATE PROCEDURE sp_phones(IN personId INT) " +
        "BEGIN " +
        "    SELECT *  " +
        "    FROM phone   " +
        "    WHERE person_id = personId;  " +
        "END"
    );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    You can retrieve the results of the aforementioned MySQL stored procedure as follows:

    </div>
    <div id="sql-jpa-call-sp-no-out-mysql-example" class="exampleblock">
    <div class="title">Example 470. Calling a MySQL stored procedure and fetching the result set without an `OUT` parameter type using JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`StoredProcedureQuery query = entityManager.createStoredProcedureQuery( "sp_phones");
    query.registerStoredProcedureParameter( 1, Long.class, ParameterMode.IN);

    query.setParameter(1, 1L);

    List&lt;Object[]&gt; personComments = query.getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-call-sp-no-out-mysql-example" class="exampleblock">
    <div class="title">Example 471. Calling a MySQL stored procedure and fetching the result set without an `OUT` parameter type using Hibernate</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session = entityManager.unwrap( Session.class );

    ProcedureCall call = session.createStoredProcedureCall( "sp_phones" );
    call.registerParameter( 1, Long.class, ParameterMode.IN ).bindValue( 1L );

    Output output = call.getOutputs().getCurrent();

    List&lt;Object[]&gt; personComments = ( (ResultSetOutput) output ).getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    For a `REF_CURSOR` result sets, we&#8217;ll consider the following Oracle stored procedure:

    </div>
    <div id="sql-sp-ref-cursor-oracle-example" class="exampleblock">
    <div class="title">Example 472. Oracle `REF_CURSOR` stored procedure</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`statement.executeUpdate(
        "CREATE OR REPLACE PROCEDURE sp_person_phones ( " +
        "   personId IN NUMBER, " +
        "   personPhones OUT SYS_REFCURSOR ) " +
        "AS  " +
        "BEGIN " +
        "    OPEN personPhones FOR " +
        "    SELECT *" +
        "    FROM phone " +
        "    WHERE person_id = personId; " +
        "END;"
    );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    `REF_CURSOR` result sets are only supported by Oracle and PostgreSQL because other database systems JDBC drivers don&#8217;t support this feature.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    This function can be called using the standard Java Persistence API:

    </div>
    <div id="sql-jpa-call-sp-ref-cursor-oracle-example" class="exampleblock">
    <div class="title">Example 473. Calling an Oracle `REF_CURSOR` stored procedure using JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`StoredProcedureQuery query = entityManager.createStoredProcedureQuery( "sp_person_phones" );
    query.registerStoredProcedureParameter( 1, Long.class, ParameterMode.IN );
    query.registerStoredProcedureParameter( 2, Class.class, ParameterMode.REF_CURSOR );
    query.setParameter( 1, 1L );

    query.execute();
    List&lt;Object[]&gt; postComments = query.getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-call-sp-ref-cursor-oracle-example" class="exampleblock">
    <div class="title">Example 474. Calling an Oracle `REF_CURSOR` stored procedure using Hibernate</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session = entityManager.unwrap(Session.class);

    ProcedureCall call = session.createStoredProcedureCall( "sp_person_phones");
    call.registerParameter(1, Long.class, ParameterMode.IN).bindValue(1L);
    call.registerParameter(2, Class.class, ParameterMode.REF_CURSOR);

    Output output = call.getOutputs().getCurrent();
    List&lt;Object[]&gt; postComments = ( (ResultSetOutput) output ).getResultList();
    assertEquals(2, postComments.size());`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    If the database defines an SQL function:

    </div>
    <div id="sql-function-mysql-example" class="exampleblock">
    <div class="title">Example 475. MySQL function</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`statement.executeUpdate(
        "CREATE FUNCTION fn_count_phones(personId integer)  " +
        "RETURNS integer " +
        "DETERMINISTIC " +
        "READS SQL DATA " +
        "BEGIN " +
        "    DECLARE phoneCount integer; " +
        "    SELECT COUNT(*) INTO phoneCount " +
        "    FROM phone  " +
        "    WHERE phone.person_id = personId; " +
        "    RETURN phoneCount; " +
        "END"
    );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Because the current `StoredProcedureQuery` implementation doesn&#8217;t yet support SQL functions,
    we need to use the JDBC syntax.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    This limitation is acknowledged and will be addressed by the [HHH-10530](https://hibernate.atlassian.net/browse/HHH-10530) issue.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div id="sql-call-function-mysql-example" class="exampleblock">
    <div class="title">Example 476. Calling a MySQL function</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`final AtomicReference&lt;Integer&gt; phoneCount = new AtomicReference&lt;&gt;();
    Session session = entityManager.unwrap( Session.class );
    session.doWork( connection -&gt; {
        try (CallableStatement function = connection.prepareCall(
                "{ ? = call fn_count_phones(?) }" )) {
            function.registerOutParameter( 1, Types.INTEGER );
            function.setInt( 2, 1 );
            function.execute();
            phoneCount.set( function.getInt( 1 ) );
        }
    } );`</pre>
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

    Stored procedure queries cannot be paged with `setFirstResult()/setMaxResults()`.

    </div>
    <div class="paragraph">

    Since these servers can return multiple result sets and update counts,
    Hibernate will iterate the results and take the first result that is a result set as its return value, so everything else will be discarded.

    </div>
    <div class="paragraph">

    For SQL Server, if you can enable `SET NOCOUNT ON` in your procedure it will probably be more efficient, but this is not a requirement.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">