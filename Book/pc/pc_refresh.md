 ### 5.9. Refresh entity state

    <div class="paragraph">

    You can reload an entity instance and its collections at any time.

    </div>
    <div id="pc-refresh-jpa-example" class="exampleblock">
    <div class="title">Example 244. Refreshing entity state with JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = entityManager.find( Person.class, personId );

    entityManager.createQuery( "update Person set name = UPPER(name)" ).executeUpdate();

    entityManager.refresh( person );
    assertEquals("JOHN DOE", person.getName() );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-refresh-native-example" class="exampleblock">
    <div class="title">Example 245. Refreshing entity state with Hibernate API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = session.byId( Person.class ).load( personId );

    session.doWork( connection -&gt; {
        try(Statement statement = connection.createStatement()) {
            statement.executeUpdate( "UPDATE person SET name = UPPER(name)" );
        }
    } );

    session.refresh( person );
    assertEquals("JOHN DOE", person.getName() );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    One case where this is useful is when it is known that the database state has changed since the data was read.
    Refreshing allows the current database state to be pulled into the entity instance and the persistence context.

    </div>
    <div class="paragraph">

    Another case where this might be useful is when database triggers are used to initialize some of the properties of the entity.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Only the entity instance and its value type collections are refreshed unless you specify `REFRESH` as a cascade style of any associations.
    However, please note that Hibernate has the capability to handle this automatically through its notion of generated properties.
    See the discussion of non-identifier [generated attributes](#mapping-generated).

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

    Traditionally, Hibernate has been allowing detached entities to be refreshed.
    Unfortunately, JPA prohibits this practice and specifies that an `IllegalArgumentException` should be thrown instead.

    </div>
    <div class="paragraph">

    For this reason, when bootstrapping the Hibernate `SessionFactory` using the native API, the legacy detached entity refresh behavior is going to be preserved.
    On the other hand, when bootstrapping Hibernate through JPA `EntityManagerFactory` building process, detached entities are not allowed to be refreshed by default.

    </div>
    <div class="paragraph">

    However, this default behavior can be overwritten through the `hibernate.allow_refresh_detached_entity` configuration property.
    If this property is explicitly set to `true`, then you can refresh detached entities even when using the JPA bootstraps mechanism, therefore bypassing the JPA specification restriction.

    </div>
    <div class="paragraph">

    For more about the `hibernate.allow_refresh_detached_entity` configuration property,
    check out the [Configurations](#misc) section as well.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="sect3">