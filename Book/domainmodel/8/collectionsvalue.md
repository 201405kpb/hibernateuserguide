#### 2.8.2. Collections of value types

    <div class="paragraph">

    Collections of value type include basic and embeddable types.
    Collections cannot be nested, and, when used in collections, embeddable types are not allowed to define other collections.

    </div>
    <div class="paragraph">

    For collections of value types, JPA 2.0 defines the `@ElementCollection` annotation.
    The lifecycle of the value-type collection is entirely controlled by its owning entity.

    </div>
    <div class="paragraph">

    Considering the previous example mapping, when clearing the phone collection, Hibernate deletes all the associated phones.
    When adding a new element to the value type collection, Hibernate issues a new insert statement.

    </div>
    <div id="collections-value-type-collection-lifecycle-example" class="exampleblock">
    <div class="title">Example 143. Value type collection lifecycle</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`person.getPhones().clear();
    person.getPhones().add( "123-456-7890" );
    person.getPhones().add( "456-000-1234" );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`DELETE FROM Person_phones WHERE   Person_id = 1

    INSERT INTO Person_phones ( Person_id, phones )
    VALUES ( 1, '123-456-7890' )

    INSERT INTO Person_phones  (Person_id, phones)
    VALUES  ( 1, '456-000-1234' )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    If removing all elements or adding new ones is rather straightforward, removing a certain entry actually requires reconstructing the whole collection from scratch.

    </div>
    <div id="collections-value-type-collection-remove-example" class="exampleblock">
    <div class="title">Example 144. Removing collection elements</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`person.getPhones().remove( 0 );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`DELETE FROM Person_phones WHERE Person_id = 1

    INSERT INTO Person_phones ( Person_id, phones )
    VALUES ( 1, '456-000-1234' )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Depending on the number of elements, this behavior might not be efficient, if many elements need to be deleted and reinserted back into the database table.
    A workaround is to use an `@OrderColumn`, which, although not as efficient as when using the actual link table primary key, might improve the efficiency of the remove operations.

    </div>
    <div id="collections-value-type-collection-order-column-remove-example" class="exampleblock">
    <div class="title">Example 145. Removing collection elements using the order column</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@ElementCollection
    @OrderColumn(name = "order_id")
    private List&lt;String&gt; phones = new ArrayList&lt;&gt;();

    person.getPhones().remove( 0 );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`DELETE FROM Person_phones
    WHERE  Person_id = 1
           AND order_id = 1

    UPDATE Person_phones
    SET    phones = '456-000-1234'
    WHERE  Person_id = 1
           AND order_id = 0`</pre>
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

    The `@OrderColumn` column works best when removing from the tail of the collection, as it only requires a single delete statement.
    Removing from the head or the middle of the collection requires deleting the extra elements and updating the remaining ones to preserve element order.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Embeddable type collections behave the same way as value type collections.
    Adding embeddables to the collection triggers the associated insert statements and removing elements from the collection will generate delete statements.

    </div>
    <div id="collections-embeddable-type-collection-lifecycle-example" class="exampleblock">
    <div class="title">Example 146. Embeddable type collections</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;

        @ElementCollection
        private List&lt;Phone&gt; phones = new ArrayList&lt;&gt;();

        public List&lt;Phone&gt; getPhones() {
            return phones;
        }
    }

    @Embeddable
    public static class Phone {

        private String type;

        @Column(name = "`number`")
        private String number;

        public Phone() {
        }

        public Phone(String type, String number) {
            this.type = type;
            this.number = number;
        }

        public String getType() {
            return type;
        }

        public String getNumber() {
            return number;
        }
    }

    person.getPhones().add( new Phone( "landline", "028-234-9876" ) );
    person.getPhones().add( new Phone( "mobile", "072-122-9876" ) );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Person_phones ( Person_id, number, type )
    VALUES ( 1, '028-234-9876', 'landline' )

    INSERT INTO Person_phones ( Person_id, number, type )
    VALUES ( 1, '072-122-9876', 'mobile' )`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">