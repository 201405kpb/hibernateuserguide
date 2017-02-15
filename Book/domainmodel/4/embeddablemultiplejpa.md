 #### 2.4.3. JPA&#8217;s AttributeOverride

    <div class="paragraph">

    JPA defines the `@AttributeOverride` annotation to handle this scenario.

    </div>
    <div class="exampleblock">
    <div class="title">Example 83. JPA&#8217;s AttributeOverride</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Contact {

        @Id
        private Integer id;

        @Embedded
        private Name name;

        @Embedded
        @AttributeOverrides(
            @AttributeOverride(
                name = "line1",
                column = @Column( name = "home_address_line1" ),
            ),
            @AttributeOverride(
                name = "line2",
                column = @Column( name = "home_address_line2" )
            ),
            @AttributeOverride(
                name = "zipCode.postalCode",
                column = @Column( name = "home_address_postal_cd" )
            ),
            @AttributeOverride(
                name = "zipCode.plus4",
                column = @Column( name = "home_address_postal_plus4" )
            )
        )
        private Address homeAddress;

        @Embedded
        @AttributeOverrides(
            @AttributeOverride(
                name = "line1",
                column = @Column( name = "mailing_address_line1" ),
            ),
            @AttributeOverride(
                name = "line2",
                column = @Column( name = "mailing_address_line2" )
            ),
            @AttributeOverride(
                name = "zipCode.postalCode",
                column = @Column( name = "mailing_address_postal_cd" )
            ),
            @AttributeOverride(
                name = "zipCode.plus4",
                column = @Column( name = "mailing_address_postal_plus4" )
            )
        )
        private Address mailingAddress;

        @Embedded
        @AttributeOverrides(
            @AttributeOverride(
                name = "line1",
                column = @Column( name = "work_address_line1" ),
            ),
            @AttributeOverride(
                name = "line2",
                column = @Column( name = "work_address_line2" )
            ),
            @AttributeOverride(
                name = "zipCode.postalCode",
                column = @Column( name = "work_address_postal_cd" )
            ),
            @AttributeOverride(
                name = "zipCode.plus4",
                column = @Column( name = "work_address_postal_plus4" )
            )
        )
        private Address workAddress;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This way, the mapping conflict is resolved by setting up explicit name-based property-column type mappings.

    </div>
    </div>
    <div class="sect3">