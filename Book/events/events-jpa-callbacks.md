 ### 14.5. JPA Callbacks

    <div class="paragraph">

    JPA also defines a more limited set of callbacks through annotations.

    </div>
    <table class="tableblock frame-all grid-all spread">
    <caption class="title">Table 7. Callback annotations</caption>
    <colgroup>
    <col style="width: %;">
    <col style="width: %;">
    </colgroup>
    <thead>
    <tr>
    <th class="tableblock halign-left valign-top">Type</th>
    <th class="tableblock halign-left valign-top">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    @PrePersist
    </td>
    <td class="tableblock halign-left valign-top">

    Executed before the entity manager persist operation is actually executed or cascaded. This call is synchronous with the persist operation.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    @PreRemove
    </td>
    <td class="tableblock halign-left valign-top">

    Executed before the entity manager remove operation is actually executed or cascaded. This call is synchronous with the remove operation.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    @PostPersist
    </td>
    <td class="tableblock halign-left valign-top">

    Executed after the entity manager persist operation is actually executed or cascaded. This call is invoked after the database INSERT is executed.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    @PostRemove
    </td>
    <td class="tableblock halign-left valign-top">

    Executed after the entity manager remove operation is actually executed or cascaded. This call is synchronous with the remove operation.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    @PreUpdate
    </td>
    <td class="tableblock halign-left valign-top">

    Executed before the database UPDATE operation.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    @PostUpdate
    </td>
    <td class="tableblock halign-left valign-top">

    Executed after the database UPDATE operation.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    @PostLoad
    </td>
    <td class="tableblock halign-left valign-top">

    Executed after an entity has been loaded into the current persistence context or an entity has been refreshed.
    </td>
    </tr>
    </tbody>
    </table>
    <div class="paragraph">

    There are two available approaches defined for specifying callback handling:

    </div>
    <div class="ulist">

*   The first approach is to annotate methods on the entity itself to receive notification of particular entity life cycle event(s).
*   The second is to use a separate entity listener class.
    An entity listener is a stateless class with a no-arg constructor.
    The callback annotations are placed on a method of this class instead of the entity class.
    The entity listener class is then associated with the entity using the `javax.persistence.EntityListeners` annotation
    </div>
    <div id="events-jpa-callbacks-example" class="exampleblock">
    <div class="title">Example 343. Example of specifying JPA callbacks</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    @EntityListeners( LastUpdateListener.class )
    public static class Person {

        @Id
        private Long id;

        private String name;

        private Date dateOfBirth;

        @Transient
        private long age;

        private Date lastUpdate;

        public void setLastUpdate(Date lastUpdate) {
            this.lastUpdate = lastUpdate;
        }

        /**
         * Set the transient property at load time based on a calculation.
         * Note that a native Hibernate formula mapping is better for this purpose.
         */
        @PostLoad
        public void calculateAge() {
            age = ChronoUnit.YEARS.between( LocalDateTime.ofInstant(
                    Instant.ofEpochMilli( dateOfBirth.getTime()), ZoneOffset.UTC),
                LocalDateTime.now()
            );
        }
    }

    public static class LastUpdateListener {

        @PreUpdate
        @PrePersist
        public void setLastUpdate( Person p ) {
            p.setLastUpdate( new Date() );
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    These approaches can be mixed, meaning you can use both together.

    </div>
    <div class="paragraph">

    Regardless of whether the callback method is defined on the entity or on an entity listener, it must have a void-return signature.
    The name of the method is irrelevant as it is the placement of the callback annotations that makes the method a callback.
    In the case of callback methods defined on the entity class, the method must additionally have a no-argument signature.
    For callback methods defined on an entity listener class, the method must have a single argument signature; the type of that argument can be either `java.lang.Object` (to facilitate attachment to multiple entities) or the specific entity type.

    </div>
    <div class="paragraph">

    A callback method can throw a `RuntimeException`.
    If the callback method does throw a `RuntimeException`, then the current transaction, if any, must be rolled back.

    </div>
    <div class="paragraph">

    A callback method must not invoke `EntityManager` or `Query` methods!

    </div>
    <div class="paragraph">

    It is possible that multiple callback methods are defined for a particular lifecycle event.
    When that is the case, the defined order of execution is well defined by the JPA spec (specifically section 3.5.4):

    </div>
    <div class="ulist">

*   Any default listeners associated with the entity are invoked first, in the order they were specified in the XML. See the `javax.persistence.ExcludeDefaultListeners` annotation.
*   Next, entity listener class callbacks associated with the entity hierarchy are invoked, in the order they are defined in the `EntityListeners`.
    If multiple classes in the entity hierarchy define entity listeners, the listeners defined for a superclass are invoked before the listeners defined for its subclasses.
    See the `javax.persistence.ExcludeSuperclassListener`s annotation.
*   Lastly, callback methods defined on the entity hierarchy are invoked.
    If a callback type is annotated on both an entity and one or more of its superclasses without method overriding, both would be called, the most general superclass first.
    An entity class is also allowed to override a callback method defined in a superclass in which case the super callback would not get invoked; the overriding method would get invoked provided it is annotated.
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">