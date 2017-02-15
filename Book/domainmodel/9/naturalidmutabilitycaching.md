 #### 2.9.3. Natural Id - Mutability and Caching

    <div class="paragraph">

    A natural id may be mutable or immutable. By default the `@NaturalId` annotation marks an immutable natural id attribute.
    An immutable natural id is expected to never change its value.
    If the value(s) of the natural id attribute(s) change, `@NaturalId(mutable=true)` should be used instead.

    </div>
    <div class="exampleblock">
    <div class="title">Example 177. Mutable natural id</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Person {

        @Id
        private Integer id;

        @NaturalId( mutable = true )
        private String ssn;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Within the Session, Hibernate maintains a mapping from natural id values to entity identifiers (PK) values.
    If natural ids values changed, it is possible for this mapping to become out of date until a flush occurs.
    To work around this condition, Hibernate will attempt to discover any such pending changes and adjust them when the `load()` or `getReference()` methods are executed.
    To be clear: this is only pertinent for mutable natural ids.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    This _discovery and adjustment_ have a performance impact.
    If an application is certain that none of its mutable natural ids already associated with the Session have changed, it can disable that checking by calling `setSynchronizationEnabled(false)` (the default is true).
    This will force Hibernate to circumvent the checking of mutable natural ids.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="exampleblock">
    <div class="title">Example 178. Mutable natural id synchronization use-case</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session=...;

    Person person = session.bySimpleNaturalId( Person.class ).load( "123-45-6789" );
    person.setSsn( "987-65-4321" );

    ...

    // returns null!
    person = session.bySimpleNaturalId( Person.class )
        .setSynchronizationEnabled( false )
        .load( "987-65-4321" );

    // returns correctly!
    person = session.bySimpleNaturalId( Person.class )
        .setSynchronizationEnabled( true )
        .load( "987-65-4321" );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Not only can this NaturalId-to-PK resolution be cached in the Session, but we can also have it cached in the second-level cache if second level caching is enabled.

    </div>
    <div id="naturalid-caching" class="exampleblock">
    <div class="title">Example 179. Natural id caching</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    @NaturalIdCache
    public class Company {

        @Id
        private Integer id;

        @NaturalId
        private String taxIdentifier;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">