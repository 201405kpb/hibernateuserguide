 #### 2.8.10. Arrays as binary

    <div class="paragraph">

    By default, Hibernate will choose a BINARY type, as supported by the current `Dialect`.

    </div>
    <div id="collections-array-binary-example" class="exampleblock">
    <div class="title">Example 167. Binary arrays</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;
        private String[] phones;

        public Person() {
        }

        public Person(Long id) {
            this.id = id;
        }

        public String[] getPhones() {
            return phones;
        }

        public void setPhones(String[] phones) {
            this.phones = phones;
        }
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE Person (
        id BIGINT NOT NULL ,
        phones VARBINARY(255) ,
        PRIMARY KEY ( id )
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">
