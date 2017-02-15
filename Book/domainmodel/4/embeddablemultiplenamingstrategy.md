 #### 2.4.4. ImplicitNamingStrategy

    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    This is a Hibernate specific feature.
    Users concerned with JPA provider portability should instead prefer explicit column naming with [`@AttributeOverride`](#embeddable-multiple-jpa).

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Hibernate naming strategies are covered in detail in [Naming](#naming).
    However, for the purposes of this discussion, Hibernate has the capability to interpret implicit column names in a way that is safe for use with multiple embeddable types.

    </div>
    <div class="exampleblock">
    <div class="title">Example 84. Enabling embeddable type safe implicit naming</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`MetadataSources sources = ...;
    sources.addAnnotatedClass( Address.class );
    sources.addAnnotatedClass( Name.class );
    sources.addAnnotatedClass( Contact.class );

    Metadata metadata = sources.getMetadataBuilder().applyImplicitNamingStrategy( ImplicitNamingStrategyComponentPathImpl.INSTANCE )
    ...
    .build();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`create table Contact(
        id integer not null,
        name_firstName VARCHAR,
        name_middleName VARCHAR,
        name_lastName VARCHAR,
        homeAddress_line1 VARCHAR,
        homeAddress_line2 VARCHAR,
        homeAddress_zipCode_postalCode VARCHAR,
        homeAddress_zipCode_plus4 VARCHAR,
        mailingAddress_line1 VARCHAR,
        mailingAddress_line2 VARCHAR,
        mailingAddress_zipCode_postalCode VARCHAR,
        mailingAddress_zipCode_plus4 VARCHAR,
        workAddress_line1 VARCHAR,
        workAddress_line2 VARCHAR,
        workAddress_zipCode_postalCode VARCHAR,
        workAddress_zipCode_plus4 VARCHAR,
        ...
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Now the "path" to attributes are used in the implicit column naming.
    You could even develop your own to do special implicit naming.

    </div>
    </div>
    <div class="sect3">