 ### 5.7. Obtain an entity by natural-id

    <div class="paragraph">

    In addition to allowing to load by identifier, Hibernate allows applications to load by declared natural identifier.

    </div>
    <div id="pc-find-by-natural-id-entity-example" class="exampleblock">
    <div class="title">Example 238. Natural-id mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Book")
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
    <div class="paragraph">

    We can also opt to fetch the entity or just retrieve a reference to it when using the natural identifier loading methods.

    </div>
    <div id="pc-find-by-simple-natural-id-example" class="exampleblock">
    <div class="title">Example 239. Get entity reference by simple natural-id</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Book book = session.bySimpleNaturalId( Book.class ).getReference( isbn );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-find-by-natural-id-example" class="exampleblock">
    <div class="title">Example 240. Load entity by natural-id</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Book book = session
        .byNaturalId( Book.class )
        .using( "isbn", isbn )
        .load( );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    We can also use a Java 8 `Optional` to load an entity by its natural id:

    </div>
    <div id="pc-find-optional-by-simple-natural-id-example" class="exampleblock">
    <div class="title">Example 241. Load an Optional entity by natural-id</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Optional&lt;Book&gt; optionalBook = session
        .byNaturalId( Book.class )
        .using( "isbn", isbn )
        .loadOptional( );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Hibernate offer a consistent API for accessing persistent data by identifier or by the natural-id. Each of these defines the same two data access methods:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">getReference</dt>
    <dd>

    Should be used in cases where the identifier is assumed to exist, where non-existence would be an actual error.
    Should never be used to test existence.
    That is because this method will prefer to create and return a proxy if the data is not already associated with the Session rather than hit the database.
    The quintessential use-case for using this method is to create foreign-key based associations.

    </dd>
    <dt class="hdlist1">load</dt>
    <dd>

    Will return the persistent data associated with the given identifier value or null if that identifier does not exist.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    Each of these two methods define an overloading variant accepting a `org.hibernate.LockOptions` argument.
    Locking is discussed in a separate [chapter](#locking).

    </div>
    </div>
    <div class="sect2">