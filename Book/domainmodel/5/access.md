 #### 2.5.9. Access strategies

    <div class="paragraph">

    As a JPA provider, Hibernate can introspect both the entity attributes (instance fields) or the accessors (instance properties).
    By default, the placement of the `@Id` annotation gives the default access strategy.
    When placed on a field, Hibernate will assume field-based access.
    Place on the identifier getter, Hibernate will use property-based access.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    You should pay attention to [Java Beans specification](https://docs.oracle.com/javase/7/docs/api/java/beans/Introspector.html#decapitalize(java.lang.String)) in regard to naming properties to avoid
    issues such as [Property name beginning with at least two uppercase characters has odd functionality in HQL](https://hibernate.atlassian.net/browse/HCANN-63)!

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Embeddable types inherit the access strategy from their parent entities.

    </div>
    <div class="sect4">

    ##### Field-based access

    <div class="exampleblock">
    <div class="title">Example 101. Field-based access</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Simple {

        @Id
        private Integer id;

        public Integer getId() {
            return id;
        }

        public void setId( Integer id ) {
            this.id = id;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When using field-based access, adding other entity-level methods is much more flexible because Hibernate won&#8217;t consider those part of the persistence state.
    To exclude a field from being part of the entity persistent state, the field must be marked with the `@Transient` annotation.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Another advantage of using field-based access is that some entity attributes can be hidden from outside the entity.
    An example of such attribute is the entity `@Version` field, which must not be manipulated by the data access layer.
    With field-based access, we can simply omit the getter and the setter for this version field, and Hibernate can still leverage the optimistic concurrency control mechanism.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect4">

    ##### Property-based access

    <div class="exampleblock">
    <div class="title">Example 102. Property-based access</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Simple {

        private Integer id;

        @Id
        public Integer getId() {
            return id;
        }

        public void setId( Integer id ) {
            this.id = id;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When using property-based access, Hibernate uses the accessors for both reading and writing the entity state.
    Every other method that will be added to the entity (e.g. helper methods for synchronizing both ends of a bidirectional one-to-many association) will have to be marked with the `@Transient` annotation.

    </div>
    </div>
    <div class="sect4">

    ##### Overriding the default access strategy

    <div class="paragraph">

    The default access strategy mechanism can be overridden with the JPA `@Access` annotation.
    In the following example, the `@Version` attribute is accessed by its field and not by its getter, like the rest of entity attributes.

    </div>
    <div class="exampleblock">
    <div class="title">Example 103. Overriding access strategy</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Simple {

        private Integer id;

        @Version
        @Access( AccessType.FIELD )
        private Integer version;

        @Id
        public Integer getId() {
            return id;
        }

        public void setId( Integer id ) {
            this.id = id;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### Embeddable types and access strategy

    <div class="paragraph">

    Because embeddables are managed by their owning entities, the access strategy is therefore inherited from the entity too.
    This applies to both simple embeddable types as well as for collection of embeddables.

    </div>
    <div class="paragraph">

    The embeddable types can overrule the default implicit access strategy (inherited from the owning entity).
    In the following example, the embeddable uses property-based access, no matter what access strategy the owning entity is choosing:

    </div>
    <div class="exampleblock">
    <div class="title">Example 104. Embeddable with exclusive access strategy</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Embeddable
    @Access(AccessType.PROPERTY)
    public static class Change {

        private String path;

        private String diff;

        public Change() {}

        @Column(name = "path", nullable = false)
        public String getPath() {
            return path;
        }

        public void setPath(String path) {
            this.path = path;
        }

        @Column(name = "diff", nullable = false)
        public String getDiff() {
            return diff;
        }

        public void setDiff(String diff) {
            this.diff = diff;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The owning entity can use field-based access, while the embeddable uses property-based access as it has chosen explicitly:

    </div>
    <div class="exampleblock">
    <div class="title">Example 105. Entity including a single embeddable type</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Patch {

        @Id
        private Long id;

        @Embedded
        private Change change;
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This works also for collection of embeddable types:

    </div>
    <div class="exampleblock">
    <div class="title">Example 106. Entity including a collection of embeddable types</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Patch {

        @Id
        private Long id;

        @ElementCollection
        @CollectionTable(
            name="patch_change",
            joinColumns=@JoinColumn(name="patch_id")
        )
        @OrderColumn(name = "index_id")
        private List&lt;Change&gt; changes = new ArrayList&lt;&gt;();

        public List&lt;Change&gt; getChanges() {
            return changes;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">