#### 2.9.1. Natural Id Mapping

    <div class="paragraph">

    Natural ids are defined in terms of one or more persistent attributes.

    </div>
    <div class="exampleblock">
    <div class="title">Example 172. Natural id using single basic attribute</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Company {

        @Id
        private Integer id;

        @NaturalId
        private String taxIdentifier;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="exampleblock">
    <div class="title">Example 173. Natural id using single embedded attribute</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class PostalCarrier {

        @Id
        private Integer id;

        @NaturalId
        @Embedded
        private PostalCode postalCode;

        ...

    }

    @Embeddable
    public class PostalCode {
        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="exampleblock">
    <div class="title">Example 174. Natural id using multiple persistent attributes</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Course {

        @Id
        private Integer id;

        @NaturalId
        @ManyToOne
        private Department department;

        @NaturalId
        private String code;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">