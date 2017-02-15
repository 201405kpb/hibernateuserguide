#### 2.6.4. Composite identifiers with `@IdClass`

    <div class="paragraph">

    Modeling a composite identifier using an IdClass differs from using an EmbeddedId in that the entity defines each individual attribute making up the composition.
    The IdClass simply acts as a "shadow".

    </div>
    <div class="exampleblock">
    <div class="title">Example 111. Basic IdClass</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    @IdClass( PK.class )
    public class Login {

        @Id
        private String system;

        @Id
        private String username;

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

    Non-aggregated composite identifiers can also contain ManyToOne attributes as we saw with aggregated ones (still non-portably)

    </div>
    <div class="exampleblock">
    <div class="title">Example 112. IdClass with ManyToOne</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    @IdClass( PK.class )
    public class Login {

        @Id
        @ManyToOne
        private System system;

        @Id
        private String username;

        public static class PK implements Serializable {

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
    <div class="paragraph">

    With non-aggregated composite identifiers, Hibernate also supports "partial" generation of the composite values.

    </div>
    <div class="exampleblock">
    <div class="title">Example 113. IdClass with partial generation</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    @IdClass( PK.class )
    public class LogFile {

        @Id
        private String name;

        @Id
        private LocalDate date;

        @Id
        @GeneratedValue
        private Integer uniqueStamp;

        public static class PK implements Serializable {

            private String name;

            private LocalDate date;

            private Integer uniqueStamp;

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

    This feature exists because of a highly questionable interpretation of the JPA specification made by the SpecJ committee.
    Hibernate does not feel that JPA defines support for this, but added the feature simply to be usable in SpecJ benchmarks.
    Use of this feature may or may not be portable from a JPA perspective.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">