#### 2.6.8. Using sequences

    <div class="paragraph">

    For implementing database sequence-based identifier value generation Hibernate makes use of its `org.hibernate.id.enhanced.SequenceStyleGenerator` id generator.
    It is important to note that SequenceStyleGenerator is capable of working against databases that do not support sequences by switching to a table as the underlying backing.
    This gives Hibernate a huge degree of portability across databases while still maintaining consistent id generation behavior (versus say choosing between sequence and IDENTITY).
    This backing storage is completely transparent to the user.

    </div>
    <div class="paragraph">

    The preferred (and portable) way to configure this generator is using the JPA-defined javax.persistence.SequenceGenerator annotation.

    </div>
    <div class="paragraph">

    The simplest form is to simply request sequence generation; Hibernate will use a single, implicitly-named sequence (`hibernate_sequence`) for all such unnamed definitions.

    </div>
    <div class="exampleblock">
    <div class="title">Example 116. Unnamed sequence</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class MyEntity {

        @Id
        @GeneratedValue( generation = SEQUENCE )
        public Integer id;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Or a specifically named sequence can be requested

    </div>
    <div class="exampleblock">
    <div class="title">Example 117. Named sequence</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class MyEntity {

        @Id
        @GeneratedValue( generation = SEQUENCE, name = "my_sequence" )
        public Integer id;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Use javax.persistence.SequenceGenerator to specify additional
    configuration.

    </div>
    <div class="exampleblock">
    <div class="title">Example 118. Configured sequence</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class MyEntity {

        @Id
        @GeneratedValue( generation = SEQUENCE, name = "my_sequence" )
        @SequenceGenerator( name = "my_sequence", schema = "globals", allocationSize = 30 )
        public Integer id;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">
