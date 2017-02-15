 #### 2.3.15. Mapping Date/Time Values

    <div class="paragraph">

    Hibernate allows various Java Date/Time classes to be mapped as persistent domain model entity properties.
    The SQL standard defines three Date/Time types:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">DATE</dt>
    <dd>

    Represents a calendar date by storing years, months and days. The JDBC equivalent is `java.sql.Date`

    </dd>
    <dt class="hdlist1">TIME</dt>
    <dd>

    Represents the time of a day and it stores hours, minutes and seconds. The JDBC equivalent is `java.sql.Time`

    </dd>
    <dt class="hdlist1">TIMESTAMP</dt>
    <dd>

    It stores both a DATE and a TIME plus nanoseconds. The JDBC equivalent is `java.sql.Timestamp`

    </dd>
    </dl>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    To avoid dependencies on the `java.sql` package, it&#8217;s common to use the `java.util` or `java.time` Date/Time classes instead.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    While the `java.sql` classes define a direct association to the SQL Date/Time data types,
    the `java.util` or `java.time` properties need to explicitly mark the SQL type correlation with the `@Temporal` annotation.
    This way, a `java.util.Date` or a `java.util.Calendar` cn be mapped to either an SQL `DATE`, `TIME` or `TIMESTAMP` type.

    </div>
    <div class="paragraph">

    Considering the following entity:

    </div>
    <div id="basic-datetime-temporal-date-example" class="exampleblock">
    <div class="title">Example 44. `java.util.Date` mapped as `DATE`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "DateEvent")
    public static class DateEvent {

        @Id
        @GeneratedValue
        private Long id;

        @Column(name = "`timestamp`")
        @Temporal(TemporalType.DATE)
        private Date timestamp;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When persisting such entity:

    </div>
    <div id="basic-datetime-temporal-date-persist-example" class="exampleblock">
    <div class="title">Example 45. Persisting a `java.util.Date` mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`DateEvent dateEvent = new DateEvent( new Date() );
    entityManager.persist( dateEvent );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Hibernate generates the following INSERT statement:

    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO DateEvent ( timestamp, id )
    VALUES ( '2015-12-29', 1 )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Only the year, month and the day field were saved into the database.

    </div>
    <div class="paragraph">

    If we change the `@Temporal` type to `TIME`:

    </div>
    <div id="basic-datetime-temporal-time-example" class="exampleblock">
    <div class="title">Example 46. `java.util.Date` mapped as `TIME`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Column(name = "`timestamp`")
    @Temporal(TemporalType.TIME)
    private Date timestamp;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Hibernate will issue an INSERT statement containing the hour, minutes and seconds.

    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO DateEvent ( timestamp, id )
    VALUES ( '16:51:58', 1 )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When the `@Temporal` type is set to `TIMESTAMP`:

    </div>
    <div id="basic-datetime-temporal-timestamp-example" class="exampleblock">
    <div class="title">Example 47. `java.util.Date` mapped as `TIMESTAMP`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Column(name = "`timestamp`")
    @Temporal(TemporalType.TIMESTAMP)
    private Date timestamp;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Hibernate will include both the `DATE`, the `TIME` and the nanoseconds in the INSERT statement:

    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO DateEvent ( timestamp, id )
    VALUES ( '2015-12-29 16:54:04.544', 1 )`</pre>
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

    Just like the `java.util.Date`, the `java.util.Calendar` requires the `@Temporal` annotation in order to know what JDBC data type to be chosen: DATE, TIME or TIMESTAMP.
    If the `java.util.Date` marks a point in time, the `java.util.Calendar` takes into consideration the default Time Zone.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="sect4">

    ##### Mapping Java 8 Date/Time Values

    <div class="paragraph">

    Java 8 came with a new Date/Time API, offering support for instant dates, intervals, local and zoned Date/Time immutable instances, bundled in the `java.time` package.

    </div>
    <div class="paragraph">

    The mapping between the standard SQL Date/Time types and the supported Java 8 Date/Time class types looks as follows;

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">DATE</dt>
    <dd>

    `java.time.LocalDate`

    </dd>
    <dt class="hdlist1">TIME</dt>
    <dd>

    `java.time.LocalTime`, `java.time.OffsetTime`

    </dd>
    <dt class="hdlist1">TIMESTAMP</dt>
    <dd>

    `java.time.Instant`, `java.time.LocalDateTime`, `java.time.OffsetDateTime` and `java.time.ZonedDateTime`

    </dd>
    </dl>
    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Because the mapping between Java 8 Date/Time classes and the SQL types is implicit, there is not need to specify the `@Temporal` annotation.
    Setting it on the `java.time` classes throws the following exception:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre>org.hibernate.AnnotationException: @Temporal should only be set on a java.util.Date or java.util.Calendar property</pre>
    </div>
    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect4">

    ##### Using a specific time zone

    <div class="paragraph">

    By default, Hibernate is going to use the [`PreparedStatement.setTimestamp(int parameterIndex, java.sql.Timestamp)`](https://docs.oracle.com/javase/8/docs/api/java/sql/PreparedStatement.html#setTimestamp-int-java.sql.Timestamp-) or
    [`PreparedStatement.setTime(int parameterIndex, java.sql.Time x)`](https://docs.oracle.com/javase/8/docs/api/java/sql/PreparedStatement.html#setTime-int-java.sql.Time-) when saving a `java.sql.Timestamp` or a `java.sql.Time` property.

    </div>
    <div class="paragraph">

    When the time zone is not specified, the JDBC driver is going to use the underlying JVM default time zone, which might not be suitable if the application is used from all across the globe.
    For this reason, it is very common to use a single reference time zone (e.g. UTC) whenever saving/loading data from the database.

    </div>
    <div class="paragraph">

    One alternative would be to configure all JVMs to use the reference time zone:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">Declaratively</dt>
    <dd>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`java -Duser.timezone=UTC ...`</pre>
    </div>
    </div>
    </dd>
    <dt class="hdlist1">Programmatically</dt>
    <dd>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`TimeZone.setDefault( TimeZone.getTimeZone( "UTC" ) );`</pre>
    </div>
    </div>
    </dd>
    </dl>
    </div>
    <div class="paragraph">

    However, as explained in [this article](http://in.relation.to/2016/09/12/jdbc-time-zone-configuration-property/), this is not always practical especially for front-end nodes.
    For this reason, Hibernate offers the `hibernate.jdbc.time_zone` configuration property which can be configured:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">Declaratively, at the `SessionFactory` level</dt>
    <dd>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`settings.put(
        AvailableSettings.JDBC_TIME_ZONE,
        TimeZone.getTimeZone( "UTC" )
    );`</pre>
    </div>
    </div>
    </dd>
    <dt class="hdlist1">Programmatically, on a per `Session` basis</dt>
    <dd>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session = sessionFactory()
        .withOptions()
        .jdbcTimeZone( TimeZone.getTimeZone( "UTC" ) )
        .openSession();`</pre>
    </div>
    </div>
    </dd>
    </dl>
    </div>
    <div class="paragraph">

    With this configuration property in place, Hibernate is going to call the [`PreparedStatement.setTimestamp(int parameterIndex, java.sql.Timestamp, Calendar cal)`](https://docs.oracle.com/javase/8/docs/api/java/sql/PreparedStatement.html#setTimestamp-int-java.sql.Timestamp-java.util.Calendar-) or
    [`PreparedStatement.setTime(int parameterIndex, java.sql.Time x, Calendar cal)`](https://docs.oracle.com/javase/8/docs/api/java/sql/PreparedStatement.html#setTime-int-java.sql.Time-java.util.Calendar-), where the `java.util.Calendar` references the time zone provided via the `hibernate.jdbc.time_zone` property.

    </div>
    </div>
    </div>
    <div class="sect3">