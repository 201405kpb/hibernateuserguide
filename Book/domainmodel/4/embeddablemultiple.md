  #### 2.4.2. Multiple embeddable types

    <div class="exampleblock">
    <div class="title">Example 82. Multiple embeddable types</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Contact {

        @Id
        private Integer id;

        @Embedded
        private Name name;

        @Embedded
        private Address homeAddress;

        @Embedded
        private Address mailingAddress;

        @Embedded
        private Address workAddress;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Although from an object-oriented perspective, it&#8217;s much more convenient to work with embeddable types, this example doesn&#8217;t work as-is.
    When the same embeddable type is included multiple times in the same parent entity type, the JPA specification demands setting the associated column names explicitly.

    </div>
    <div class="paragraph">

    This requirement is due to how object properties are mapped to database columns.
    By default, JPA expects a database column having the same name with its associated object property.
    When including multiple embeddables, the implicit name-based mapping rule doesn&#8217;t work anymore because multiple object properties could end-up being mapped to the same database column.

    </div>
    <div class="paragraph">

    We have a few options to handle this issue.

    </div>
    </div>
    <div class="sect3">
