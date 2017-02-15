 #### 2.3.16. JPA 2.1 AttributeConverters

    <div class="paragraph">

    Although Hibernate has long been offering [custom types](#basic-custom-type), as a JPA 2.1 provider,
    it also supports `AttributeConverter`s as well.

    </div>
    <div class="paragraph">

    With a custom `AttributeConverter`, the application developer can map a given JDBC type to an entity basic type.

    </div>
    <div class="paragraph">

    In the following example, the `java.util.Period` is going to be mapped to a `VARCHAR` database column.

    </div>
    <div id="basic-jpa-convert-period-string-converter-example" class="exampleblock">
    <div class="title">Example 48. `java.util.Period` custom `AttributeConverter`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Converter
    public class PeriodStringConverter
            implements AttributeConverter&lt;Period, String&gt; {

        @Override
        public String convertToDatabaseColumn(Period attribute) {
            return attribute.toString();
        }

        @Override
        public Period convertToEntityAttribute(String dbData) {
            return Period.parse( dbData );
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To make use of this custom converter, the `@Convert` annotation must decorate the entity attribute.

    </div>
    <div id="basic-jpa-convert-period-string-converter-mapping-example" class="exampleblock">
    <div class="title">Example 49. Entity using the custom `java.util.Period` `AttributeConverter` mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Event")
    public static class Event {

        @Id
        @GeneratedValue
        private Long id;

        @Convert(converter = PeriodStringConverter.class)
        @Column(columnDefinition = "")
        private Period span;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When persisting such entity, Hibernate will do the type conversion based on the `AttributeConverter` logic:

    </div>
    <div id="basic-jpa-convert-period-string-converter-sql-example" class="exampleblock">
    <div class="title">Example 50. Persisting entity using the custom `AttributeConverter`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Event ( span, id )
    VALUES ( 'P1Y2M3D', 1 )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### JPA 2.1 `AttributeConverter` Mutability Plan

    <div class="paragraph">

    A basic type that&#8217;s converted by a JPA `AttributeConverter` is immutable if the underlying Java type is immutable
    and is mutable if the associated attribute type is mutable as well.

    </div>
    <div class="paragraph">

    Therefore, mutability is given by the [`JavaTypeDescriptor#getMutabilityPlan`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/type/descriptor/java/JavaTypeDescriptor.html#getMutabilityPlan--)
    of the associated entity attribute type.

    </div>
    <div class="sect5">

    ###### Immutable types

    <div class="paragraph">

    If the entity attribute is a `String`, a primitive wrapper (e.g. `Integer`, `Long`) an Enum type, or any other immutable `Object` type,
    then you can only change the entity attribute value by reassigning it to a new value.

    </div>
    <div class="paragraph">

    Considering we have the same `Period` entity attribute as illustrated in the [JPA 2.1 AttributeConverters](#basic-jpa-convert) section:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Event")
    public static class Event {

        @Id
        @GeneratedValue
        private Long id;

        @Convert(converter = PeriodStringConverter.class)
        @Column(columnDefinition = "")
        private Period span;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    <div class="paragraph">

    The only way to change the `span` attribute is to reassign it to a different value:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Event event = entityManager.createQuery( "from Event", Event.class ).getSingleResult();
    event.setSpan(Period
        .ofYears( 3 )
        .plusMonths( 2 )
        .plusDays( 1 )
    );`</pre>
    </div>
    </div>
    </div>
    <div class="sect5">

    ###### Mutable types

    <div class="paragraph">

    On the other hand, consider the following example where the `Money` type is a mutable.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public static class Money {

        private long cents;

        public Money(long cents) {
            this.cents = cents;
        }

        public long getCents() {
            return cents;
        }

        public void setCents(long cents) {
            this.cents = cents;
        }
    }

    public static class MoneyConverter
            implements AttributeConverter&lt;Money, Long&gt; {

        @Override
        public Long convertToDatabaseColumn(Money attribute) {
            return attribute == null ? null : attribute.getCents();
        }

        @Override
        public Money convertToEntityAttribute(Long dbData) {
            return dbData == null ? null : new Money( dbData );
        }
    }

    @Entity(name = "Account")
    public static class Account {

        @Id
        private Long id;

        private String owner;

        @Convert(converter = MoneyConverter.class)
        private Money balance;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    <div class="paragraph">

    A mutable `Object` allows you to modify its internal structure, and Hibernate dirty checking mechanism is going to propagate the change to the database:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Account account = entityManager.find( Account.class, 1L );
    account.getBalance().setCents( 150 * 100L );
    entityManager.persist( account );`</pre>
    </div>
    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Although the `AttributeConverter` types can be mutable so that dirty checking, deep copying and second-level caching work properly,
    treating these as immutable (when they really are) is more efficient.

    </div>
    <div class="paragraph">

    For this reason, prefer immutable types over mutable ones whenever possible.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">