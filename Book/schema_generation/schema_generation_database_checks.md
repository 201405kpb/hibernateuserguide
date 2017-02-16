### 4.3. Database-level checks

    <div class="paragraph">

    Hibernate offers the `@Check` annotation so that you can specify an arbitrary SQL CHECK constraint which can be defined as follows:

    </div>
    <div id="schema-generation-database-checks-example" class="exampleblock">
    <div class="title">Example 217. Database check entity mapping example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Book")
    @Check( constraints = "CASE WHEN isbn IS NOT NULL THEN LENGTH(isbn) = 13 ELSE true END")
    public static class Book {

        @Id
        private Long id;

        private String title;

        @NaturalId
        private String isbn;

        private Double price;

        //Getters and setters omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Now, if you try to add a `Book` entity with an `isbn` attribute whose length is not 13 characters,
    a `ConstraintViolationException` is going to be thrown.

    </div>
    <div id="stag::schema-generation-database-checks-persist-example" class="exampleblock">
    <div class="title">Example 218. Database check failure example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Book book = new Book();
    book.setId( 1L );
    book.setPrice( 49.99d );
    book.setTitle( "High-Performance Java Persistence" );
    book.setIsbn( "11-11-2016" );

    entityManager.persist( book );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT  INTO Book (isbn, price, title, id)
    VALUES  ('11-11-2016', 49.99, 'High-Performance Java Persistence', 1)

    -- WARN SqlExceptionHelper:129 - SQL Error: 0, SQLState: 23514
    -- ERROR SqlExceptionHelper:131 - ERROR: new row for relation "book" violates check constraint "book_isbn_check"`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">
