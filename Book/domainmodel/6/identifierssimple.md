 #### 2.6.1. Simple identifiers

    <div class="paragraph">

    Simple identifiers map to a single basic attribute, and are denoted using the `javax.persistence.Id` annotation.

    </div>
    <div class="paragraph">

    According to JPA only the following types should be used as identifier attribute types:

    </div>
    <div class="ulist">

*   any Java primitive type
*   any primitive wrapper type
*   `java.lang.String`
*   `java.util.Date` (TemporalType#DATE)
*   `java.sql.Date`
*   `java.math.BigDecimal`
*   `java.math.BigInteger`
    </div>
    <div class="paragraph">

    Any types used for identifier attributes beyond this list will not be portable.

    </div>
    <div class="sect4">

    ##### Assigned identifiers

    <div class="paragraph">

    Values for simple identifiers can be assigned, as we have seen in the examples above.
    The expectation for assigned identifier values is that the application assigns (sets them on the entity attribute) prior to calling save/persist.

    </div>
    <div class="exampleblock">
    <div class="title">Example 107. Simple assigned identifier</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class MyEntity {

        @Id
        public Integer id;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### Generated identifiers

    <div class="paragraph">

    Values for simple identifiers can be generated. To denote that an identifier attribute is generated, it is annotated with `javax.persistence.GeneratedValue`

    </div>
    <div class="exampleblock">
    <div class="title">Example 108. Simple generated identifier</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class MyEntity {

        @Id
        @GeneratedValue
        public Integer id;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Additionally, to the type restriction list above, JPA says that if using generated identifier values (see below) only integer types (short, int, long) will be portably supported.

    </div>
    <div class="paragraph">

    The expectation for generated identifier values is that Hibernate will generate the value when the save/persist occurs.

    </div>
    <div class="paragraph">

    Identifier value generations strategies are discussed in detail in the [Generated identifier values](#identifiers-generators) section.

    </div>
    </div>
    </div>
    <div class="sect3">