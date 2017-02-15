 #### 2.8.11. Collections as basic value type

    <div class="paragraph">

    Notice how all the previous examples explicitly mark the collection attribute as either `ElementCollection`, `OneToMany` or `ManyToMany`.
    Collections not marked as such require a custom Hibernate `Type` and the collection elements must be stored in a single database column.

    </div>
    <div class="paragraph">

    This is sometimes beneficial. Consider a use-case such as a `VARCHAR` column that represents a delimited list/set of Strings.

    </div>
    <div id="collections-comma-delimited-collection-example" class="exampleblock">
    <div class="title">Example 168. Comma delimited collection</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;

        @Type(type = "comma_delimited_strings")
        private List&lt;String&gt; phones = new ArrayList&lt;&gt;();

        public List&lt;String&gt; getPhones() {
            return phones;
        }
    }

    public class CommaDelimitedStringsJavaTypeDescriptor extends AbstractTypeDescriptor&lt;List&gt; {

        public static final String DELIMITER = ",";

        public CommaDelimitedStringsJavaTypeDescriptor() {
            super(
                List.class,
                new MutableMutabilityPlan&lt;List&gt;() {
                    @Override
                    protected List deepCopyNotNull(List value) {
                        return new ArrayList( value );
                    }
                }
            );
        }

        @Override
        public String toString(List value) {
            return ( (List&lt;String&gt;) value ).stream().collect( Collectors.joining( DELIMITER ) );
        }

        @Override
        public List fromString(String string) {
            List&lt;String&gt; values = new ArrayList&lt;&gt;();
            Collections.addAll( values, string.split( DELIMITER ) );
            return values;
        }

        @Override
        public &lt;X&gt; X unwrap(List value, Class&lt;X&gt; type, WrapperOptions options) {
            return (X) toString( value );
        }

        @Override
        public &lt;X&gt; List wrap(X value, WrapperOptions options) {
            return fromString( (String) value );
        }
    }

    public class CommaDelimitedStringsType extends AbstractSingleColumnStandardBasicType&lt;List&gt; {

        public CommaDelimitedStringsType() {
            super(
                VarcharTypeDescriptor.INSTANCE,
                new CommaDelimitedStringsJavaTypeDescriptor()
            );
        }

        @Override
        public String getName() {
            return "comma_delimited_strings";
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The developer can use the comma-delimited collection like any other collection we&#8217;ve discussed so far and Hibernate will take care of the type transformation part.
    The collection itself behaves like any other basic value type, as its lifecycle is bound to its owner entity.

    </div>
    <div id="collections-comma-delimited-collection-lifecycle-example" class="exampleblock">
    <div class="title">Example 169. Comma delimited collection lifecycle</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`person.phones.add( "027-123-4567" );
    person.phones.add( "028-234-9876" );
    session.flush();
    person.getPhones().remove( 0 );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Person ( phones, id )
    VALUES ( '027-123-4567,028-234-9876', 1 )

    UPDATE Person
    SET    phones = '028-234-9876'
    WHERE  id = 1`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    See the Hibernate Integrations Guide for more details on developing custom value type mappings.

    </div>
    </div>
    <div class="sect3">