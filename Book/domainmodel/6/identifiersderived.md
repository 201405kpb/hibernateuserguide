 #### 2.6.14. Derived Identifiers

    <div class="paragraph">

    JPA 2.0 added support for derived identifiers which allow an entity to borrow the identifier from a many-to-one or one-to-one association.

    </div>
    <div id="identifiers-derived-mapsid" class="exampleblock">
    <div class="title">Example 123. Derived identifier with `@MapsId`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Person  {

        @Id
        @GeneratedValue
        private Long id;

        @NaturalId
        private String registrationNumber;

        public Person() {}

        public Person(String registrationNumber) {
            this.registrationNumber = registrationNumber;
        }

        public Long getId() {
            return id;
        }

        public String getRegistrationNumber() {
            return registrationNumber;
        }
    }

    @Entity
    public class PersonDetails  {

        @Id
        private Long id;

        private String nickName;

        @ManyToOne
        @MapsId
        private Person person;

        public String getNickName() {
            return nickName;
        }

        public void setNickName(String nickName) {
            this.nickName = nickName;
        }

        public Person getPerson() {
            return person;
        }

        public void setPerson(Person person) {
            this.person = person;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In the example above, the `PersonDetails` entity uses the `id` column for both the entity identifier and for the many-to-one association to the `Person` entity.
    The value of the `PersonDetails` entity identifier is "derived" from the identifier of its parent `Person` entity.
    The `@MapsId` annotation can also reference columns from an `@EmbeddedId` identifier as well.

    </div>
    <div class="paragraph">

    The previous example can also be mapped using `@PrimaryKeyJoinColumn`.

    </div>
    <div id="identifiers-derived-primarykeyjoincolumn" class="exampleblock">
    <div class="title">Example 124. Derived identifier `@PrimaryKeyJoinColumn`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class PersonDetails  {

        @Id
        private Long id;

        private String nickName;

        @ManyToOne
        @PrimaryKeyJoinColumn
        private Person person;

        public String getNickName() {
            return nickName;
        }

        public void setNickName(String nickName) {
            this.nickName = nickName;
        }

        public Person getPerson() {
            return person;
        }

        public void setPerson(Person person) {
            this.person = person;
            this.id = person.getId();
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

    Unlike `@MapsId`, the application developer is responsible for ensuring that the identifier and the many-to-one (or one-to-one) association are in sync.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    <div class="sect2">