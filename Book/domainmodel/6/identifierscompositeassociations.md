 #### 2.6.5. Composite identifiers with associations

    <div class="paragraph">

    Hibernate allows defining a composite identifier out of entity associations.
    In the following example, the `PersonAddress` entity identifier is formed of two `@ManyToOne` associations.

    </div>
    <div class="exampleblock">
    <div class="title">Example 114. Composite identifiers with associations</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class PersonAddress  implements Serializable {

        @Id
        @ManyToOne
        private Person person;

        @Id
        @ManyToOne()
        private Address address;

        public PersonAddress() {}

        public PersonAddress(Person person, Address address) {
            this.person = person;
            this.address = address;
        }

        public Person getPerson() {
            return person;
        }

        public void setPerson(Person person) {
            this.person = person;
        }

        public Address getAddress() {
            return address;
        }

        public void setAddress(Address address) {
            this.address = address;
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            PersonAddress that = (PersonAddress) o;
            return Objects.equals(person, that.person) &amp;&amp;
                    Objects.equals(address, that.address);
        }

        @Override
        public int hashCode() {
            return Objects.hash(person, address);
        }
    }

    @Entity
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

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            Person person = (Person) o;
            return Objects.equals(registrationNumber, person.registrationNumber);
        }

        @Override
        public int hashCode() {
            return Objects.hash(registrationNumber);
        }
    }

    @Entity
    public class Address  {

        @Id
        @GeneratedValue
        private Long id;

        private String street;

        private String number;

        private String postalCode;

        public Address() {}

        public Address(String street, String number, String postalCode) {
            this.street = street;
            this.number = number;
            this.postalCode = postalCode;
        }

        public Long getId() {
            return id;
        }

        public String getStreet() {
            return street;
        }

        public String getNumber() {
            return number;
        }

        public String getPostalCode() {
            return postalCode;
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            Address address = (Address) o;
            return Objects.equals(street, address.street) &amp;&amp;
                    Objects.equals(number, address.number) &amp;&amp;
                    Objects.equals(postalCode, address.postalCode);
        }

        @Override
        public int hashCode() {
            return Objects.hash(street, number, postalCode);
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Although the mapping is much simpler than using an `@EmbeddedId` or an `@IdClass`, there&#8217;s no separation between the entity instance and the actual identifier.
    To query this entity, an instance of the entity itself must be supplied to the persistence context.

    </div>
    <div class="exampleblock">
    <div class="title">Example 115. Composite identifiers with associations query</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`PersonAddress personAddress = entityManager.find(
        PersonAddress.class,
        new PersonAddress( person, address )
    );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">