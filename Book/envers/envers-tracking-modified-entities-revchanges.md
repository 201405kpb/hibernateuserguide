### 21.6. Tracking entity names modified during revisions

    <div class="paragraph">

    By default entity types that have been changed in each revision are not being tracked.
    This implies the necessity to query all tables storing audited data in order to retrieve changes made during specified revision.
    Envers provides a simple mechanism that creates `REVCHANGES` table which stores entity names of modified persistent objects.
    Single record encapsulates the revision identifier (foreign key to `REVINFO` table) and a string value.

    </div>
    <div class="paragraph">

    Tracking of modified entity names can be enabled in three different ways:

    </div>
    <div class="olist arabic">

1.  Set `org.hibernate.envers.track_entities_changed_in_revision` parameter to `true`.
    In this case `org.hibernate.envers.DefaultTrackingModifiedEntitiesRevisionEntity` will be implicitly used as the revision log entity.
2.  Create a custom revision entity that extends `org.hibernate.envers.DefaultTrackingModifiedEntitiesRevisionEntity` class.
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@RevisionEntity
    public class ExtendedRevisionEntity extends DefaultTrackingModifiedEntitiesRevisionEntity {
    	...
    }`</pre>
    </div>
    </div>
3.  Mark an appropriate field of a custom revision entity with `@org.hibernate.envers.ModifiedEntityNames` annotation.
    The property is required to be of `Set&lt;String&gt;` type.
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@RevisionEntity
    public class AnnotatedTrackingRevisionEntity {
        ...
        @ElementCollection
        @JoinTable( name = "REVCHANGES", joinColumns = @JoinColumn( name = "REV" ) )
        @Column( name = "ENTITYNAME" )
        @ModifiedEntityNames
        private Set&lt;String&gt; modifiedEntityNames;
        ...
    }`</pre>
    </div>
    </div>
    <div class="paragraph">
    Users, that have chosen one of the approaches listed above,
    can retrieve all entities modified in a specified revision by utilizing API described in [Querying for entities modified in a given revision](#envers-tracking-modified-entities-queries).
    </div>
    </div>
    <div class="paragraph">

    Users are also allowed to implement custom mechanism of tracking modified entity types.
    In this case, they shall pass their own implementation of `org.hibernate.envers.EntityTrackingRevisionListener` interface as the value of `@org.hibernate.envers.RevisionEntity` annotation.
    `EntityTrackingRevisionListener` interface exposes one method that notifies whenever audited entity instance has been added, modified or removed within current revision boundaries.

    </div>
    <div class="exampleblock">
    <div class="title">Example 498. CustomEntityTrackingRevisionListener.java</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public class CustomEntityTrackingRevisionListener implements EntityTrackingRevisionListener {

        @Override
        public void entityChanged( Class entityClass, String entityName,
                                   Serializable entityId, RevisionType revisionType,
                                   Object revisionEntity ) {
            String type = entityClass.getName();
            ( ( CustomTrackingRevisionEntity ) revisionEntity ).addModifiedEntityType( type );
        }

        @Override
        public void newRevision( Object revisionEntity ) {
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="exampleblock">
    <div class="title">Example 499. CustomTrackingRevisionEntity.java</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    @RevisionEntity( CustomEntityTrackingRevisionListener.class )
    public class CustomTrackingRevisionEntity {

        @Id
        @GeneratedValue
        @RevisionNumber
        private int customId;

        @RevisionTimestamp
        private long customTimestamp;

        @OneToMany( mappedBy="revision", cascade={ CascadeType.PERSIST, CascadeType.REMOVE } )
        private Set&lt;ModifiedEntityTypeEntity&gt; modifiedEntityTypes = new HashSet&lt;ModifiedEntityTypeEntity&gt;();

        public void addModifiedEntityType( String entityClassName ) {
            modifiedEntityTypes.add( new ModifiedEntityTypeEntity( this, entityClassName ) );
        }

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="exampleblock">
    <div class="title">Example 500. ModifiedEntityTypeEntity.java</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class ModifiedEntityTypeEntity {

        @Id
        @GeneratedValue
        private Integer id;

        @ManyToOne
        private CustomTrackingRevisionEntity revision;

        private String entityClassName;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CustomTrackingRevisionEntity revEntity =
        getAuditReader().findRevision( CustomTrackingRevisionEntity.class, revisionNumber );

    Set&lt;ModifiedEntityTypeEntity&gt; modifiedEntityTypes = revEntity.getModifiedEntityTypes();`</pre>
    </div>
    </div>
    </div>
    <div class="sect2">