 #### 2.7.3. `@OneToOne`

    <div class="paragraph">

    The `@OneToOne` association can either be unidirectional or bidirectional.
    A unidirectional association follows the relational database foreign key semantics, the client-side owning the relationship.
    A bidirectional association features a `mappedBy` `@OneToOne` parent side too.

    </div>
    <div class="sect4">

    ##### Unidirectional `@OneToOne`

    <div id="associations-one-to-one-unidirectional-example" class="exampleblock">
    <div class="title">Example 131. Unidirectional `@OneToOne`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Phone")
    public static class Phone {

        @Id
        @GeneratedValue
        private Long id;

        @Column(name = "`number`")
        private String number;

        @OneToOne
        @JoinColumn(name = "details_id")
        private PhoneDetails details;

        public Phone() {
        }

        public Phone(String number) {
            this.number = number;
        }

        public Long getId() {
            return id;
        }

        public String getNumber() {
            return number;
        }

        public PhoneDetails getDetails() {
            return details;
        }

        public void setDetails(PhoneDetails details) {
            this.details = details;
        }
    }

    @Entity(name = "PhoneDetails")
    public static class PhoneDetails {

        @Id
        @GeneratedValue
        private Long id;

        private String provider;

        private String technology;

        public PhoneDetails() {
        }

        public PhoneDetails(String provider, String technology) {
            this.provider = provider;
            this.technology = technology;
        }

        public String getProvider() {
            return provider;
        }

        public String getTechnology() {
            return technology;
        }

        public void setTechnology(String technology) {
            this.technology = technology;
        }
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Phone (
        id BIGINT NOT NULL ,
        number VARCHAR(255) ,
        details_id BIGINT ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE PhoneDetails (
        id BIGINT NOT NULL ,
        provider VARCHAR(255) ,
        technology VARCHAR(255) ,
        PRIMARY KEY ( id )
    )

    ALTER TABLE Phone
    ADD CONSTRAINT FKnoj7cj83ppfqbnvqqa5kolub7
    FOREIGN KEY (details_id) REFERENCES PhoneDetails`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    From a relational database point of view, the underlying schema is identical to the unidirectional `@ManyToOne` association,
    as the client-side controls the relationship based on the foreign key column.

    </div>
    <div class="paragraph">

    But then, it&#8217;s unusual to consider the `Phone` as a client-side and the `PhoneDetails` as the parent-side because the details cannot exist without an actual phone.
    A much more natural mapping would be if the `Phone` were the parent-side, therefore pushing the foreign key into the `PhoneDetails` table.
    This mapping requires a bidirectional `@OneToOne` association as you can see in the following example:

    </div>
    </div>
    <div class="sect4">

    ##### Bidirectional `@OneToOne`

    <div id="associations-one-to-one-bidirectional-example" class="exampleblock">
    <div class="title">Example 132. Bidirectional `@OneToOne`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Phone")
    public static class Phone {

        @Id
        @GeneratedValue
        private Long id;

        @Column(name = "`number`")
        private String number;

        @OneToOne(mappedBy = "phone", cascade = CascadeType.ALL, orphanRemoval = true, fetch = FetchType.LAZY)
        private PhoneDetails details;

        public Phone() {
        }

        public Phone(String number) {
            this.number = number;
        }

        public Long getId() {
            return id;
        }

        public String getNumber() {
            return number;
        }

        public PhoneDetails getDetails() {
            return details;
        }

        public void addDetails(PhoneDetails details) {
            details.setPhone( this );
            this.details = details;
        }

        public void removeDetails() {
            if ( details != null ) {
                details.setPhone( null );
                this.details = null;
            }
        }
    }

    @Entity(name = "PhoneDetails")
    public static class PhoneDetails {

        @Id
        @GeneratedValue
        private Long id;

        private String provider;

        private String technology;

        @OneToOne(fetch = FetchType.LAZY)
        @JoinColumn(name = "phone_id")
        private Phone phone;

        public PhoneDetails() {
        }

        public PhoneDetails(String provider, String technology) {
            this.provider = provider;
            this.technology = technology;
        }

        public String getProvider() {
            return provider;
        }

        public String getTechnology() {
            return technology;
        }

        public void setTechnology(String technology) {
            this.technology = technology;
        }

        public Phone getPhone() {
            return phone;
        }

        public void setPhone(Phone phone) {
            this.phone = phone;
        }
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Phone (
        id BIGINT NOT NULL ,
        number VARCHAR(255) ,
        PRIMARY KEY ( id )
    )

    CREATE TABLE PhoneDetails (
        id BIGINT NOT NULL ,
        provider VARCHAR(255) ,
        technology VARCHAR(255) ,
        phone_id BIGINT ,
        PRIMARY KEY ( id )
    )

    ALTER TABLE PhoneDetails
    ADD CONSTRAINT FKeotuev8ja8v0sdh29dynqj05p
    FOREIGN KEY (phone_id) REFERENCES Phone`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This time, the `PhoneDetails` owns the association, and, like any bidirectional association, the parent-side can propagate its lifecycle to the child-side through cascading.

    </div>
    <div id="associations-one-to-one-bidirectional-lifecycle-example" class="exampleblock">
    <div class="title">Example 133. Bidirectional `@OneToOne` lifecycle</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Phone phone = new Phone( "123-456-7890" );
    PhoneDetails details = new PhoneDetails( "T-Mobile", "GSM" );

    phone.addDetails( details );
    entityManager.persist( phone );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Phone ( number, id )
    VALUES ( '123 - 456 - 7890', 1 )

    INSERT INTO PhoneDetails ( phone_id, provider, technology, id )
    VALUES ( 1, 'T - Mobile, GSM', 2 )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When using a bidirectional `@OneToOne` association, Hibernate enforces the unique constraint upon fetching the child-side.
    If there are more than one children associated with the same parent, Hibernate will throw a constraint violation exception.
    Continuing the previous example, when adding another `PhoneDetails`, Hibernate validates the uniqueness constraint when reloading the `Phone` object.

    </div>
    <div id="associations-one-to-one-bidirectional-constraint-example" class="exampleblock">
    <div class="title">Example 134. Bidirectional `@OneToOne` unique constraint</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`PhoneDetails otherDetails = new PhoneDetails( "T-Mobile", "CDMA" );
    otherDetails.setPhone( phone );
    entityManager.persist( otherDetails );
    entityManager.flush();
    entityManager.clear();

    //throws javax.persistence.PersistenceException: org.hibernate.HibernateException: More than one row with the given identifier was found: 1
    phone = entityManager.find( Phone.class, phone.getId() );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">
