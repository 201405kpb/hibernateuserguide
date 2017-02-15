 # Mapping enums
Hibernate supports the mapping of Java enums as basic value types in a number of different ways.
##### `@Enumerated`

 The original JPA-compliant way to map enums was via the `@Enumerated` and `@MapKeyEnumerated` for map keys annotations which works on the principle that the enum values are stored according to one of 2 strategies indicated by `javax.persistence.EnumType`:

 `ORDINAL`
    - stored according to the enum value&#8217;s ordinal position within the enum class, as indicated by java.lang.Enum#ordinal
`STRING`
    - stored according to the enum value&#8217;s name, as indicated by java.lang.Enum#name

    Assuming the following enumeration:

    In the ORDINAL example, the `phone_type` column is defined as an (nullable) INTEGER type and would hold:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`NULL`</dt>
    <dd>

    For null values

    </dd>
    <dt class="hdlist1">`0`</dt>
    <dd>

    For the `LAND_LINE` enum

    </dd>
    <dt class="hdlist1">`1`</dt>
    <dd>

    For the `MOBILE` enum

    </dd>
    </dl>
    </div>
    <div id="basic-enums-Enumerated-ordinal-example" class="exampleblock">
    <div class="title">Example 18. `@Enumerated(ORDINAL)` example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Phone")
    public static class Phone {

        @Id
        private Long id;

        @Column(name = "phone_number")
        private String number;

        @Enumerated(EnumType.ORDINAL)
        @Column(name = "phone_type")
        private PhoneType type;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When persisting this entity, Hibernate generates the following SQL statement:

    </div>
    <div id="basic-enums-Enumerated-ordinal-persistence-example" class="exampleblock">
    <div class="title">Example 19. Persisting an entity with an `@Enumerated(ORDINAL)` mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Phone phone = new Phone( );
    phone.setId( 1L );
    phone.setNumber( "123-456-78990" );
    phone.setType( PhoneType.MOBILE );
    entityManager.persist( phone );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Phone (phone_number, phone_type, id)
    VALUES ('123-456-78990', 2, 1)`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In the STRING example, the `phone_type` column is defined as an (nullable) VARCHAR type and would hold:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`NULL`</dt>
    <dd>

    For null values

    </dd>
    <dt class="hdlist1">`LAND_LINE`</dt>
    <dd>

    For the `LAND_LINE` enum

    </dd>
    <dt class="hdlist1">`MOBILE`</dt>
    <dd>

    For the `MOBILE` enum

    </dd>
    </dl>
    </div>
    <div id="basic-enums-Enumerated-string-example" class="exampleblock">
    <div class="title">Example 20. `@Enumerated(STRING)` example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Phone")
    public static class Phone {

        @Id
        private Long id;

        @Column(name = "phone_number")
        private String number;

        @Enumerated(EnumType.STRING)
        @Column(name = "phone_type")
        private PhoneType type;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Persisting the same entity like in the `@Enumerated(ORDINAL)` example, Hibernate generates the following SQL statement:

    </div>
    <div id="basic-enums-Enumerated-string-persistence-example" class="exampleblock">
    <div class="title">Example 21. Persisting an entity with an `@Enumerated(STRING)` mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Phone (phone_number, phone_type, id)
    VALUES ('123-456-78990', 'MOBILE', 1)`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### AttributeConverter

    <div class="paragraph">

    Let&#8217;s consider the following `Gender` enum which stores its values using the `'M'` and `'F'` codes.

    </div>
    <div id="basic-enums-converter-example" class="exampleblock">
    <div class="title">Example 22. Enum with custom constructor</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public enum Gender {

        MALE( 'M' ),
        FEMALE( 'F' );

        private final char code;

        Gender(char code) {
            this.code = code;
        }

        public static Gender fromCode(char code) {
            if ( code == 'M' || code == 'm' ) {
                return MALE;
            }
            if ( code == 'F' || code == 'f' ) {
                return FEMALE;
            }
            throw new UnsupportedOperationException(
                "The code " + code + " is not supported!"
            );
        }

        public char getCode() {
            return code;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    You can map enums in a JPA compliant way using a JPA 2.1 AttributeConverter.

    </div>
    <div id="basic-enums-attribute-converter-example" class="exampleblock">
    <div class="title">Example 23. Enum mapping with `AttributeConverter` example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;

        private String name;

        @Convert( converter = GenderConverter.class )
        public Gender gender;

        //Getters and setters are omitted for brevity

    }

    @Converter
    public static class GenderConverter
            implements AttributeConverter&lt;Gender, Character&gt; {

        public Character convertToDatabaseColumn( Gender value ) {
            if ( value == null ) {
                return null;
            }

            return value.getCode();
        }

        public Gender convertToEntityAttribute( Character value ) {
            if ( value == null ) {
                return null;
            }

            return Gender.fromCode( value );
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Here, the gender column is defined as a CHAR type and would hold:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`NULL`</dt>
    <dd>

    For null values

    </dd>
    <dt class="hdlist1">`'M'`</dt>
    <dd>

    For the `MALE` enum

    </dd>
    <dt class="hdlist1">`'F'`</dt>
    <dd>

    For the `FEMALE` enum

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    For additional details on using AttributeConverters, see [JPA 2.1 AttributeConverters](#basic-jpa-convert) section.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    JPA explicitly disallows the use of an AttributeConverter with an attribute marked as `@Enumerated`.
    So if using the AttributeConverter approach, be sure not to mark the attribute as `@Enumerated`.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect4">

    ##### Custom type

    <div class="paragraph">

    You can also map enums using a Hibernate custom type mapping.
    Let&#8217;s again revisit the Gender enum example, this time using a custom Type to store the more standardized `'M'` and `'F'` codes.

    </div>
    <div id="basic-enums-custom-type-example" class="exampleblock">
    <div class="title">Example 24. Enum mapping with custom Type example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;

        private String name;

        @Type( type = "org.hibernate.userguide.mapping.basic.GenderType" )
        public Gender gender;

        //Getters and setters are omitted for brevity

    }

    public class GenderType extends AbstractSingleColumnStandardBasicType&lt;Gender&gt; {

        public static final GenderType INSTANCE = new GenderType();

        public GenderType() {
            super(
                CharTypeDescriptor.INSTANCE,
                GenderJavaTypeDescriptor.INSTANCE
            );
        }

        public String getName() {
            return "gender";
        }

        @Override
        protected boolean registerUnderJavaType() {
            return true;
        }
    }

    public class GenderJavaTypeDescriptor extends AbstractTypeDescriptor&lt;Gender&gt; {

        public static final GenderJavaTypeDescriptor INSTANCE =
            new GenderJavaTypeDescriptor();

        protected GenderJavaTypeDescriptor() {
            super( Gender.class );
        }

        public String toString(Gender value) {
            return value == null ? null : value.name();
        }

        public Gender fromString(String string) {
            return string == null ? null : Gender.valueOf( string );
        }

        public &lt;X&gt; X unwrap(Gender value, Class&lt;X&gt; type, WrapperOptions options) {
            return CharacterTypeDescriptor.INSTANCE.unwrap(
                value == null ? null : value.getCode(),
                type,
                options
            );
        }

        public &lt;X&gt; Gender wrap(X value, WrapperOptions options) {
            return Gender.fromCode(
                CharacterTypeDescriptor.INSTANCE.wrap( value, options )
            );
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Again, the gender column is defined as a CHAR type and would hold:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`NULL`</dt>
    <dd>

    For null values

    </dd>
    <dt class="hdlist1">`'M'`</dt>
    <dd>

    For the `MALE` enum

    </dd>
    <dt class="hdlist1">`'F'`</dt>
    <dd>

    For the `FEMALE` enum

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    For additional details on using custom types, see [Custom BasicTypes](#basic-custom-type) section.

    </div>
    </div>
    </div>
    <div class="sect3">
