    #### 5.2.1. Capabilities

    <div class="paragraph">

    Hibernate supports the enhancement of an application Java domain model for the purpose of adding various persistence-related capabilities directly into the class.

    </div>
    <div class="sect4">
##### Lazy attribute loading

    <div class="paragraph">

    Think of this as partial loading support.
    Essentially you can tell Hibernate that only part(s) of an entity should be loaded upon fetching from the database and when the other part(s) should be loaded as well.
    Note that this is very much different from proxy-based idea of lazy loading which is entity-centric where the entity&#8217;s state is loaded at once as needed.
    With bytecode enhancement, individual attributes or groups of attributes are loaded as needed.

    </div>
    <div class="paragraph">

    Lazy attributes can be designated to be loaded together and this is called a "lazy group".
    By default, all singular attributes are part of a single group, meaning that when one lazy singular attribute is accessed all lazy singular attributes are loaded.
    Lazy plural attributes, by default, are each a lazy group by themselves.
    This behavior is explicitly controllable through the `@org.hibernate.annotations.LazyGroup` annotation.

    </div>
    <div id="BytecodeEnhancement-lazy-loading-example" class="exampleblock">
    <div class="title">Example 222. `@LazyGroup` example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Customer {

        @Id
        private Integer id;

        private String name;

        @Basic( fetch = FetchType.LAZY )
        private UUID accountsPayableXrefId;

        @Lob
        @Basic( fetch = FetchType.LAZY )
        @LazyGroup( "lobs" )
        private Blob image;

        public Integer getId() {
            return id;
        }

        public void setId(Integer id) {
            this.id = id;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public UUID getAccountsPayableXrefId() {
            return accountsPayableXrefId;
        }

        public void setAccountsPayableXrefId(UUID accountsPayableXrefId) {
            this.accountsPayableXrefId = accountsPayableXrefId;
        }

        public Blob getImage() {
            return image;
        }

        public void setImage(Blob image) {
            this.image = image;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In the above example we have 2 lazy attributes: `accountsPayableXrefId` and `image`.
    Each is part of a different fetch group (accountsPayableXrefId is part of the default fetch group),
    which means that accessing `accountsPayableXrefId` will not force the loading of image, and vice-versa.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    As a hopefully temporary legacy hold-over, it is currently required that all lazy singular associations (many-to-one and one-to-one) also include `@LazyToOne(LazyToOneOption.NO_PROXY)`.
    The plan is to relax that requirement later.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect4">

    ##### In-line dirty tracking

    <div class="paragraph">

    Historically Hibernate only supported diff-based dirty calculation for determining which entities in a persistence context have changed.
    This essentially means that Hibernate would keep track of the last known state of an entity in regards to the database (typically the last read or write).
    Then, as part of flushing the persistence context, Hibernate would walk every entity associated with the persistence context and check its current state against that "last known database state".
    This is by far the most thorough approach to dirty checking because it accounts for data-types that can change their internal state (`java.util.Date` is the prime example of this).
    However, in a persistence context with a large number of associated entities it can also be a performance-inhibiting approach.

    </div>
    <div class="paragraph">

    If your application does not need to care about "internal state changing data-type" use cases, bytecode-enhanced dirty tracking might be a worthwhile alternative to consider, especially in terms of performance.
    In this approach Hibernate will manipulate the bytecode of your classes to add "dirty tracking" directly to the entity, allowing the entity itself to keep track of which of its attributes have changed.
    During flush time, Hibernate simply asks your entity what has changed rather that having to perform the state-diff calculations.

    </div>
    </div>
    <div class="sect4">

    ##### Bidirectional association management

    <div class="paragraph">

    Hibernate strives to keep your application as close to "normal Java usage" (idiomatic Java) as possible.
    Consider a domain model with a normal `Person`/`Book` bidirectional association:

    </div>
    <div id="BytecodeEnhancement-dirty-tracking-bidirectional-example" class="exampleblock">
    <div class="title">Example 223. Bidirectional association</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;

        private String name;

        @OneToMany(mappedBy = "author")
        private List&lt;Book&gt; books = new ArrayList&lt;&gt;(  );

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public List&lt;Book&gt; getBooks() {
            return books;
        }
    }

    @Entity(name = "Book")
    public static class Book {

        @Id
        private Long id;

        private String title;

        @NaturalId
        private String isbn;

        @ManyToOne
        private Person author;

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        public String getTitle() {
            return title;
        }

        public void setTitle(String title) {
            this.title = title;
        }

        public Person getAuthor() {
            return author;
        }

        public void setAuthor(Person author) {
            this.author = author;
        }

        public String getIsbn() {
            return isbn;
        }

        public void setIsbn(String isbn) {
            this.isbn = isbn;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="BytecodeEnhancement-dirty-tracking-bidirectional-incorrect-usage-example" class="exampleblock">
    <div class="title">Example 224. Incorrect normal Java usage</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person();
    person.setName( "John Doe" );

    Book book = new Book();
    person.getBooks().add( book );
    try {
        book.getAuthor().getName();
    }
    catch (NullPointerException expected) {
        // This blows up ( NPE ) in normal Java usage
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This blows up in normal Java usage. The correct normal Java usage is:

    </div>
    <div id="BytecodeEnhancement-dirty-tracking-bidirectional-correct-usage-example" class="exampleblock">
    <div class="title">Example 225. Correct normal Java usage</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person();
    person.setName( "John Doe" );

    Book book = new Book();
    person.getBooks().add( book );
    book.setAuthor( person );

    book.getAuthor().getName();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Bytecode-enhanced bi-directional association management makes that first example work by managing the "other side" of a bi-directional association whenever one side is manipulated.

    </div>
    </div>
    <div class="sect4">

    ##### Internal performance optimizations

    <div class="paragraph">

    Additionally, we use the enhancement process to add some additional code that allows us to optimized certain performance characteristics of the persistence context.
    These are hard to discuss without diving into a discussion of Hibernate internals.

    </div>
    </div>
    </div>
    <div class="sect3">