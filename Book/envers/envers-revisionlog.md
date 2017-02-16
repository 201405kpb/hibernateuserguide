 ### 21.5. Revision Log

    <div class="paragraph">

    When Envers starts a new revision, it creates a new revision entity which stores information about the revision.
    By default, that includes just:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">revision number</dt>
    <dd>

    An integral value (`int/Integer` or `long/Long`). Essentially the primary key of the revision

    </dd>
    <dt class="hdlist1">revision timestamp</dt>
    <dd>

    either a `long/Long` or `java.util.Date` value representing the instant at which the revision was made.
    When using a `java.util.Date`, instead of a `long/Long` for the revision timestamp, take care not to store it to a column data type which will loose precision.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    Envers handles this information as an entity.
    By default it uses its own internal class to act as the entity, mapped to the `REVINFO` table.
    You can, however, supply your own approach to collecting this information which might be useful to capture additional details such as who made a change or the ip address from which the request came.
    There are two things you need to make this work:

    </div>
    <div class="olist arabic">

1.  First, you will need to tell Envers about the entity you wish to use.
    Your entity must use the `@org.hibernate.envers.RevisionEntity` annotation.
    It must define the two attributes described above annotated with `@org.hibernate.envers.RevisionNumber` and `@org.hibernate.envers.RevisionTimestamp`, respectively.
    You can extend from `org.hibernate.envers.DefaultRevisionEntity`, if you wish, to inherit all these required behaviors.
    <div class="literalblock">
    <div class="content">
    <pre>Simply add the custom revision entity as you do your normal entities and Envers will _find it_.</pre>
    </div>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">
    </td>
    <td class="content">
    It is an error for there to be multiple entities marked as `@org.hibernate.envers.RevisionEntity`
    </td>
    </tr>
    </table>
    </div>
2.  Second, you need to tell Envers how to create instances of your revision entity which is handled by the [`newRevision( Object revisionEntity )`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/envers/RevisionListener.html#newRevision-java.lang.Object-) method of the `org.hibernate.envers.RevisionListener` interface.
    <div class="literalblock">
    <div class="content">
    <pre>You tell Envers your custom `org.hibernate.envers.RevisionListener` implementation to use by specifying it on the `@org.hibernate.envers.RevisionEntity` annotation, using the value attribute.
    If your `RevisionListener` class is inaccessible from `@RevisionEntity` (e.g. it exists in a different module), set `org.hibernate.envers.revision_listener` property to its fully qualified class name.
    Class name defined by the configuration parameter overrides revision entity's value attribute.</pre>
    </div>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@RevisionEntity( MyCustomRevisionListener.class )
    public class MyCustomRevisionEntity {
        ...
    }

    public class MyCustomRevisionListener implements RevisionListener {
        public void newRevision( Object revisionEntity ) {
            MyCustomRevisionEntity customRevisionEntity = ( MyCustomRevisionEntity ) revisionEntity;
        }
    }`</pre>
    </div>
    </div>
    <div class="exampleblock">
    <div class="title">Example 496. ExampleRevEntity.java</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`package `org.hibernate.envers.example;`

    import `org.hibernate.envers.RevisionEntity;`
    import `org.hibernate.envers.DefaultRevisionEntity;`

    import javax.persistence.Entity;

    @Entity
    @RevisionEntity( ExampleListener.class )
    public class ExampleRevEntity extends DefaultRevisionEntity {
        private String username;

        public String getUsername() { return username; }
        public void setUsername( String username ) { this.username = username; }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="exampleblock">
    <div class="title">Example 497. ExampleListener.java</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`package `org.hibernate.envers.example;`

    import `org.hibernate.envers.RevisionListener;`
    import org.jboss.seam.security.Identity;
    import org.jboss.seam.Component;

    public class ExampleListener implements RevisionListener {

        public void newRevision( Object revisionEntity ) {
            ExampleRevEntity exampleRevEntity = ( ExampleRevEntity ) revisionEntity;
            Identity identity =
                (Identity) Component.getInstance( "org.jboss.seam.security.identity" );

            exampleRevEntity.setUsername( identity.getUsername() );
        }
    }`</pre>
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

    An alternative method to using the `org.hibernate.envers.RevisionListener` is to instead call the [`getCurrentRevision( Class&lt;T&gt; revisionEntityClass, boolean persist )`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/envers/AuditReader.html#getCurrentRevision-java.lang.Class-boolean-) method of the `org.hibernate.envers.AuditReader` interface to obtain the current revision, and fill it with desired information.
    The method accepts a `persist` parameter indicating whether the revision entity should be persisted prior to returning from this method:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`true`</dt>
    <dd>

    ensures that the returned entity has access to its identifier value (revision number), but the revision entity will be persisted regardless of whether there are any audited entities changed.

    </dd>
    <dt class="hdlist1">`false`</dt>
    <dd>

    means that the revision number will be `null`, but the revision entity will be persisted only if some audited entities have changed.

    </dd>
    </dl>
    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">