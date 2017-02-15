#### 2.6.3. Composite identifiers with `@EmbeddedId`

    <div class="paragraph">

    Modeling a composite identifier using an EmbeddedId simply means defining an embeddable to be a composition for the one or more attributes making up the identifier,
    and then exposing an attribute of that embeddable type on the entity.

    </div>
    <div class="exampleblock">
    <div class="title">Example 109. Basic EmbeddedId</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Login {

        @EmbeddedId
        private PK pk;

        @Embeddable
        public static class PK implements Serializable {

            private String system;

            private String username;

            ...
        }

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    As mentioned before, EmbeddedIds can even contain ManyToOne attributes.

    </div>
    <div class="exampleblock">
    <div class="title">Example 110. EmbeddedId with ManyToOne</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Login {

        @EmbeddedId
        private PK pk;

        @Embeddable
        public static class PK implements Serializable {

            @ManyToOne
            private System system;

            private String username;

            ...
        }

        ...
    }`</pre>
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

    Hibernate supports directly modeling the ManyToOne in the PK class, whether EmbeddedId or IdClass.
    However that is not portably supported by the JPA specification.
    In JPA terms one would use "derived identifiers"; for details, see [Derived Identifiers](#identifiers-derived).

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">