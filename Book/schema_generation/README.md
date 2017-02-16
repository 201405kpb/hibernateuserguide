 ## 4. Schema generation

    <div class="sectionbody">
    <div class="paragraph">

    Hibernate allows you to generate the database from the entity mappings.

    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Although the automatic schema generation is very useful for testing and prototyping purposes, in a production environment,
    it&#8217;s much more flexible to manage the schema using incremental migration scripts.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Traditionally, the process of generating  schema from entity mapping has been called `HBM2DDL`.
    To get a list of Hibernate-native and JPA-specific configuration properties consider reading the [Configurations](#configurations-hbmddl) section.

    </div>
    <div class="paragraph">

    Considering the following Domain Model:

    </div>
    <div id="schema-generation-domain-model-example" class="exampleblock">
    <div class="title">Example 212. Schema generation Domain Model</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Customer")
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
    }

    @Entity(name = "Person")
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
    <div class="paragraph">

    If the `hibernate.hbm2ddl.auto` configuration is set to `create`, Hibernate is going to generate the following database schema:

    </div>
    <div id="sql-schema-generation-domain-model-example" class="exampleblock">
    <div class="title">Example 213. Auto-generated database schema</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`create table Customer (
        id integer not null,
        accountsPayableXrefId binary,
        image blob,
        name varchar(255),
        primary key (id)
    )

    create table Book (
        id bigint not null,
        isbn varchar(255),
        title varchar(255),
        author_id bigint,
        primary key (id)
    )

    create table Person (
        id bigint not null,
        name varchar(255),
        primary key (id)
    )

    alter table Book
        add constraint UK_u31e1frmjp9mxf8k8tmp990i unique (isbn)

    alter table Book
        add constraint FKrxrgiajod1le3gii8whx2doie
        foreign key (author_id)
        references Person`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">