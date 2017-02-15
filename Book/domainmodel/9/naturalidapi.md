#### 2.9.2. Natural Id API

    <div class="paragraph">

    As stated before, Hibernate provides an API for loading entities by their associate natural id.
    This is represented by the `org.hibernate.NaturalIdLoadAccess` contract obtained via Session#byNaturalId.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    If the entity does not define a natural id, trying to load an entity by its natural id will throw an exception.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="exampleblock">
    <div class="title">Example 175. Using NaturalIdLoadAccess</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session = ...;

    Company company = session.byNaturalId( Company.class )
        .using( "taxIdentifier","abc-123-xyz" )
        .load();

    PostalCarrier carrier = session.byNaturalId( PostalCarrier.class )
        .using( "postalCode",new PostalCode(... ) )
        .load();

    Department department = ...;
    Course course = session.byNaturalId( Course.class )
        .using( "department",department )
        .using( "code","101" )
        .load();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    NaturalIdLoadAccess offers 2 distinct methods for obtaining the entity:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`load()`</dt>
    <dd>

    obtains a reference to the entity, making sure that the entity state is initialized

    </dd>
    <dt class="hdlist1">`getReference()`</dt>
    <dd>

    obtains a reference to the entity. The state may or may not be initialized.
    If the entity is already associated with the current running Session, that reference (loaded or not) is returned.
    If the entity is not loaded in the current Session and the entity supports proxy generation, an uninitialized proxy is generated and returned, otherwise the entity is loaded from the database and returned.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    `NaturalIdLoadAccess` allows loading an entity by natural id and at the same time apply a pessimistic lock.
    For additional details on locking, see the [Locking](#locking) chapter.

    </div>
    <div class="paragraph">

    We will discuss the last method available on NaturalIdLoadAccess ( `setSynchronizationEnabled()` ) in [Natural Id - Mutability and Caching](#naturalid-mutability-caching).

    </div>
    <div class="paragraph">

    Because the `Company` and `PostalCarrier` entities define "simple" natural ids, we can load them as follows:

    </div>
    <div class="exampleblock">
    <div class="title">Example 176. Using SimpleNaturalIdLoadAccess</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session = ...;

    Company company = session.bySimpleNaturalId( Company.class )
        .load( "abc-123-xyz" );

    PostalCarrier carrier = session.bySimpleNaturalId( PostalCarrier.class )
        .load( new PostalCode(... ) );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Here we see the use of the `org.hibernate.SimpleNaturalIdLoadAccess` contract, obtained via `Session#bySimpleNaturalId().
    `SimpleNaturalIdLoadAccess` is similar to `NaturalIdLoadAccess` except that it does not define the using method.
    Instead, because these "simple" natural ids are defined based on just one attribute we can directly pass the corresponding value of that natural id attribute directly to the `load()` and `getReference()` methods.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    If the entity does not define a natural id, or if the natural id is not of a "simple" type, an exception will be thrown there.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">