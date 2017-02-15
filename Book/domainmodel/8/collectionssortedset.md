#### 2.8.7. Sorted sets

    <div class="paragraph">

    For sorted sets, the entity mapping must use the `SortedSet` interface instead.
    According to the `SortedSet` contract, all elements must implement the comparable interface and therefore provide the sorting logic.

    </div>
    <div class="sect4">

    ##### Unidirectional sorted sets

    <div class="paragraph">

    A `SortedSet` that relies on the natural sorting order given by the child element `Comparable` implementation logic must be annotated with the `@SortNatural` Hibernate annotation.

    </div>
    <div id="collections-unidirectional-sorted-set-natural-comparator-example" class="exampleblock">
    <div class="title">Example 160. Unidirectional natural sorted set</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;
        @OneToMany(cascade = CascadeType.ALL)
        @SortNatural
        private SortedSet&lt;Phone&gt; phones = new TreeSet&lt;&gt;();

        public Person() {
        }

        public Person(Long id) {
            this.id = id;
        }

        public Set&lt;Phone&gt; getPhones() {
            return phones;
        }
    }

    @Entity(name = "Phone")
    public static class Phone implements Comparable&lt;Phone&gt; {

        @Id
        private Long id;

        private String type;

        @NaturalId
        @Column(name = "`number`")
        private String number;

        public Phone() {
        }

        public Phone(Long id, String type, String number) {
            this.id = id;
            this.type = type;
            this.number = number;
        }

        public Long getId() {
            return id;
        }

        public String getType() {
            return type;
        }

        public String getNumber() {
            return number;
        }

        @Override
        public int compareTo(Phone o) {
            return number.compareTo( o.getNumber() );
        }

        @Override
        public boolean equals(Object o) {
            if ( this == o ) {
                return true;
            }
            if ( o == null || getClass() != o.getClass() ) {
                return false;
            }
            Phone phone = (Phone) o;
            return Objects.equals( number, phone.number );
        }

        @Override
        public int hashCode() {
            return Objects.hash( number );
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The lifecycle and the database mapping are identical to the [Unidirectional bags](#collections-unidirectional-bag), so they are intentionally omitted.

    </div>
    <div class="paragraph">

    To provide a custom sorting logic, Hibernate also provides a `@SortComparator` annotation:

    </div>
    <div id="collections-unidirectional-sorted-set-custom-comparator-example" class="exampleblock">
    <div class="title">Example 161. Unidirectional custom comparator sorted set</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;

        @OneToMany(cascade = CascadeType.ALL)
        @SortComparator(ReverseComparator.class)
        private SortedSet&lt;Phone&gt; phones = new TreeSet&lt;&gt;();

        public Person() {
        }

        public Person(Long id) {
            this.id = id;
        }

        public Set&lt;Phone&gt; getPhones() {
            return phones;
        }
    }

    public static class ReverseComparator implements Comparator&lt;Phone&gt; {
        @Override
        public int compare(Phone o1, Phone o2) {
            return o2.compareTo( o1 );
        }
    }

    @Entity(name = "Phone")
    public static class Phone implements Comparable&lt;Phone&gt; {

        @Id
        private Long id;

        private String type;

        @NaturalId
        @Column(name = "`number`")
        private String number;

        public Phone() {
        }

        public Phone(Long id, String type, String number) {
            this.id = id;
            this.type = type;
            this.number = number;
        }

        public Long getId() {
            return id;
        }

        public String getType() {
            return type;
        }

        public String getNumber() {
            return number;
        }

        @Override
        public int compareTo(Phone o) {
            return number.compareTo( o.getNumber() );
        }

        @Override
        public boolean equals(Object o) {
            if ( this == o ) {
                return true;
            }
            if ( o == null || getClass() != o.getClass() ) {
                return false;
            }
            Phone phone = (Phone) o;
            return Objects.equals( number, phone.number );
        }

        @Override
        public int hashCode() {
            return Objects.hash( number );
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### Bidirectional sorted sets

    <div class="paragraph">

    The `@SortNatural` and `@SortComparator` work the same for bidirectional sorted sets too:

    </div>
    <div id="collections-bidirectional-sorted-set-example" class="exampleblock">
    <div class="title">Example 162. Bidirectional natural sorted set</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@OneToMany(mappedBy = "person", cascade = CascadeType.ALL)
    @SortNatural
    private SortedSet&lt;Phone&gt; phones = new TreeSet&lt;&gt;();

    @OneToMany(cascade = CascadeType.ALL)
    @SortComparator(ReverseComparator.class)`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">
