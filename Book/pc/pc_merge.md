 #### 5.10.2. Merging detached data

    <div class="paragraph">

    Merging is the process of taking an incoming entity instance that is in detached state and copying its data over onto a new managed instance.

    </div>
    <div class="paragraph">

    Although not exactly per se, the following example is a good visualization of the `merge` operation internals.

    </div>
    <div id="pc-merge-visualize-example" class="exampleblock">
    <div class="title">Example 249. Visualizing merge</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public Person merge(Person detached) {
        Person newReference = session.byId( Person.class ).load( detached.getId() );
        newReference.setName( detached.getName() );
        return newReference;
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-merge-jpa-example" class="exampleblock">
    <div class="title">Example 250. Merging a detached entity with JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = entityManager.find( Person.class, personId );
    //Clear the EntityManager so the person entity becomes detached
    entityManager.clear();
    person.setName( "Mr. John Doe" );

    person = entityManager.merge( person );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-merge-native-example" class="exampleblock">
    <div class="title">Example 251. Merging a detached entity with Hibernate API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = session.byId( Person.class ).load( personId );
    //Clear the Session so the person entity becomes detached
    session.clear();
    person.setName( "Mr. John Doe" );

    person = (Person) session.merge( person );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### Merging gotchas

    <div class="paragraph">

    For example, Hibernate throws `IllegalStateException` when merging a parent entity which has references to 2 detached child entities `child1` and `child2` (obtained from different sessions), and `child1` and `child2` represent the same persistent entity, `Child`.

    </div>
    <div class="paragraph">

    A new configuration property, `hibernate.event.merge.entity_copy_observer`, controls how Hibernate will respond when multiple representations of the same persistent entity ("entity copy") is detected while merging.

    </div>
    <div class="paragraph">

    The possible values are:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">disallow (the default)</dt>
    <dd>

    throws `IllegalStateException` if an entity copy is detected

    </dd>
    <dt class="hdlist1">allow</dt>
    <dd>

    performs the merge operation on each entity copy that is detected

    </dd>
    <dt class="hdlist1">log</dt>
    <dd>

    (provided for testing only) performs the merge operation on each entity copy that is detected and logs information about the entity copies.
    This setting requires DEBUG logging be enabled for `org.hibernate.event.internal.EntityCopyAllowedLoggedObserver`.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    In addition, the application may customize the behavior by providing an implementation of `org.hibernate.event.spi.EntityCopyObserver` and setting `hibernate.event.merge.entity_copy_observer` to the class name.
    When this property is set to `allow` or `log`, Hibernate will merge each entity copy detected while cascading the merge operation.
    In the process of merging each entity copy, Hibernate will cascade the merge operation from each entity copy to its associations with `cascade=CascadeType.MERGE` or `CascadeType.ALL`.
    The entity state resulting from merging an entity copy will be overwritten when another entity copy is merged.

    </div>
    <div class="admonitionblock warning">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Because cascade order is undefined, the order in which the entity copies are merged is undefined.
    As a result, if property values in the entity copies are not consistent, the resulting entity state will be indeterminate, and data will be lost from all entity copies except for the last one merged.
    Therefore, the **last writer wins**.

    </div>
    <div class="paragraph">

    If an entity copy cascades the merge operation to an association that is (or contains) a new entity, that new entity will be merged (i.e., persisted and the merge operation will be cascaded to its associations according to its mapping),
    even if that same association is ultimately overwritten when Hibernate merges a different representation having a different value for its association.

    </div>
    <div class="paragraph">

    If the association is mapped with `orphanRemoval = true`, the new entity will not be deleted because the semantics of orphanRemoval do not apply if the entity being orphaned is a new entity.

    </div>
    <div class="paragraph">

    There are known issues when representations of the same persistent entity have different values for a collection.
    See [HHH-9239](https://hibernate.atlassian.net/browse/HHH-9239) and [HHH-9240](https://hibernate.atlassian.net/browse/HHH-9240) for more details.
    These issues can cause data loss or corruption.

    </div>
    <div class="paragraph">

    By setting `hibernate.event.merge.entity_copy_observer` configuration property to `allow` or `log`,
    Hibernate will allow entity copies of any type of entity to be merged.

    </div>
    <div class="paragraph">

    The only way to exclude particular entity classes or associations that contain critical data is to provide a custom implementation of `org.hibernate.event.spi.EntityCopyObserver` with the desired behavior,
    and setting `hibernate.event.merge.entity_copy_observer` to the class name.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Hibernate provides limited DEBUG logging capabilities that can help determine the entity classes for which entity copies were found.
    By setting `hibernate.event.merge.entity_copy_observer` to `log` and enabling DEBUG logging for `org.hibernate.event.internal.EntityCopyAllowedLoggedObserver`,
    the following will be logged each time an application calls `EntityManager.merge( entity )` or

    `Session.merge( entity )`:

    </div>
    <div class="ulist">

*   number of times multiple representations of the same persistent entity was detected summarized by entity name;
*   details by entity name and ID, including output from calling toString() on each representation being merged as well as the merge result.
    </div>
    <div class="paragraph">

    The log should be reviewed to determine if multiple representations of entities containing critical data are detected.
    If so, the application should be modified so there is only one representation, and a custom implementation of `org.hibernate.event.spi.EntityCopyObserver` should be provided to disallow entity copies for entities with critical data.

    </div>
    <div class="paragraph">

    Using optimistic locking is recommended to detect if different representations are from different versions of the same persistent entity.
    If they are not from the same version, Hibernate will throw either the JPA `OptimisticLockException` or the native `StaleObjectStateException` depending on your bootstrapping strategy.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">