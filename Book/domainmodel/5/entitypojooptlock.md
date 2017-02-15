#### 2.5.8. Mapping optimistic locking

    <div class="paragraph">

    JPA defines support for optimistic locking based on either a version (sequential numeric) or timestamp strategy.
    To enable this style of optimistic locking simply add the `javax.persistence.Version` to the persistent attribute that defines the optimistic locking value.
    According to JPA, the valid types for these attributes are limited to:

    </div>
    <div class="ulist">

*   `int` or `Integer`
*   `short` or `Short`
*   `long` or `Long`
*   `java.sql.Timestamp`
    </div>
    <div id="entity-pojo-optlock-version-example" class="exampleblock">
    <div class="title">Example 96. `@Version` annotation mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Course {

        @Id
        private Integer id;

        @Version
        private Integer version;
        ...
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Thing {

        @Id
        private Integer id;

        @Version
        private Timestamp ts;

        ...
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Thing2 {

        @Id
        private Integer id;

        @Version
        private Instant ts;
        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### Versionless optimistic locking

    <div class="paragraph">

    Although the default `@Version` property optimistic locking mechanism is sufficient in many situations,
    sometimes, you need rely on the actual database row column values to prevent **lost updates**.

    </div>
    <div class="paragraph">

    Hibernate supports a form of optimistic locking that does not require a dedicated "version attribute".
    This is also useful for use with modeling legacy schemas.

    </div>
    <div class="paragraph">

    The idea is that you can get Hibernate to perform "version checks" using either all of the entity&#8217;s attributes, or just the attributes that have changed.
    This is achieved through the use of the
    [`@OptimisticLocking`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/OptimisticLocking.html)
    annotation which defines a single attribute of type
    [`org.hibernate.annotations.OptimisticLockType`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/OptimisticLockType.html).

    </div>
    <div class="paragraph">

    There are 4 available OptimisticLockTypes:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`NONE`</dt>
    <dd>

    optimistic locking is disabled even if there is a `@Version` annotation present

    </dd>
    <dt class="hdlist1">`VERSION` (the default)</dt>
    <dd>

    performs optimistic locking based on a `@Version` as described above

    </dd>
    <dt class="hdlist1">`ALL`</dt>
    <dd>

    performs optimistic locking based on _all_ fields as part of an expanded WHERE clause restriction for the UPDATE/DELETE SQL statements

    </dd>
    <dt class="hdlist1">`DIRTY`</dt>
    <dd>

    performs optimistic locking based on _dirty_ fields as part of an expanded WHERE clause restriction for the UPDATE/DELETE SQL statements.

    </dd>
    </dl>
    </div>
    <div class="sect5">

    ###### Versionless optimistic locking using `OptimisticLockType.ALL`

    <div id="locking-optimistic-lock-type-all-example" class="exampleblock">
    <div class="title">Example 97. `OptimisticLockType.ALL` mapping example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    @OptimisticLocking(type = OptimisticLockType.ALL)
    @DynamicUpdate
    public static class Person {

        @Id
        private Long id;

        @Column(name = "`name`")
        private String name;

        private String country;

        private String city;

        @Column(name = "created_on")
        private Timestamp createdOn;

        //Getters and setters are omitted for brevity
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When you need to modify the `Person` entity above:

    </div>
    <div id="locking-optimistic-lock-type-all-update-example" class="exampleblock">
    <div class="title">Example 98. `OptimisticLockType.ALL` update example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = entityManager.find( Person.class, 1L );
    person.setCity( "Washington D.C." );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`UPDATE
        Person
    SET
        city=?
    WHERE
        id=?
        AND city=?
        AND country=?
        AND created_on=?
        AND "name"=?

    -- binding parameter [1] as [VARCHAR] - [Washington D.C.]
    -- binding parameter [2] as [BIGINT] - [1]
    -- binding parameter [3] as [VARCHAR] - [New York]
    -- binding parameter [4] as [VARCHAR] - [US]
    -- binding parameter [5] as [TIMESTAMP] - [2016-11-16 16:05:12.876]
    -- binding parameter [6] as [VARCHAR] - [John Doe]`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    As you can see, all the columns of the associated database row are used in the `WHERE` clause.
    If any column has changed after the row was loaded, there won&#8217;t be any match, and a `StaleStateException` or an `OptimisticLockException`
    is going to be thrown.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    When using `OptimisticLockType.ALL`, you should also use `@DynamicUpdate` because the `UPDATE` statement must take into consideration all the entity property values.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect5">

    ###### Versionless optimistic locking using `OptimisticLockType.DIRTY`

    <div class="paragraph">

    The `OptimisticLockType.DIRTY` differs from `OptimisticLockType.ALL`
    in that it only takes into consideration the entity properties that have changed
    since the entity was loaded in the currently running Persistence Context.

    </div>
    <div id="locking-optimistic-lock-type-dirty-example" class="exampleblock">
    <div class="title">Example 99. `OptimisticLockType.DIRTY` mapping example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    @OptimisticLocking(type = OptimisticLockType.DIRTY)
    @DynamicUpdate
    @SelectBeforeUpdate
    public static class Person {

        @Id
        private Long id;

        @Column(name = "`name`")
        private String name;

        private String country;

        private String city;

        @Column(name = "created_on")
        private Timestamp createdOn;

        //Getters and setters are omitted for brevity
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When you need to modify the `Person` entity above:

    </div>
    <div id="locking-optimistic-lock-type-dirty-update-example" class="exampleblock">
    <div class="title">Example 100. `OptimisticLockType.DIRTY` update example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = entityManager.find( Person.class, 1L );
    person.setCity( "Washington D.C." );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`UPDATE
        Person
    SET
        city=?
    WHERE
        id=?
        and city=?

    -- binding parameter [1] as [VARCHAR] - [Washington D.C.]
    -- binding parameter [2] as [BIGINT] - [1]
    -- binding parameter [3] as [VARCHAR] - [New York]`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This time, only the database column that has changed was used in the `WHERE` clause.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The main advantage of `OptimisticLockType.DIRTY` over `OptimisticLockType.ALL`
    and the default `OptimisticLockType.VERSION` used implicitly along with the `@Version` mapping,
    is that it allows you to minimize the risk of `OptimisticLockException` across non-overlapping entity property changes.

    </div>
    <div class="paragraph">

    When using `OptimisticLockType.DIRTY`, you should also use `@DynamicUpdate` because the `UPDATE` statement must take into consideration all the dirty entity property values,
    and also the `@SelectBeforeUpdate` annotation so that detached entities are properly handled by the
    [`Session#update(entity)`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/Session.html#update-java.lang.Object-) operation.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">