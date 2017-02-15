#### 2.6.11. Using UUID generation

    <div class="paragraph">

    As mentioned above, Hibernate supports UUID identifier value generation.
    This is supported through its `org.hibernate.id.UUIDGenerator` id generator.

    </div>
    <div class="paragraph">

    `UUIDGenerator` supports pluggable strategies for exactly how the UUID is generated.
    These strategies are defined by the `org.hibernate.id.UUIDGenerationStrategy` contract.
    The default strategy is a version 4 (random) strategy according to IETF RFC 4122.
    Hibernate does ship with an alternative strategy which is a RFC 4122 version 1 (time-based) strategy (using ip address rather than mac address).

    </div>
    <div class="exampleblock">
    <div class="title">Example 121. Implicitly using the random UUID strategy</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class MyEntity {

        @Id
        @GeneratedValue
        public UUID id;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To specify an alternative generation strategy, we&#8217;d have to define some configuration via `@GenericGenerator`.
    Here we choose the RFC 4122 version 1 compliant strategy named `org.hibernate.id.uuid.CustomVersionOneStrategy`

    </div>
    <div class="exampleblock">
    <div class="title">Example 122. Implicitly using the random UUID strategy</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class MyEntity {

        @Id
        @GeneratedValue( generator = "uuid" )
        @GenericGenerator(
            name = "uuid",
            strategy = "org.hibernate.id.UUIDGenerator",
            parameters = {
                @Parameter(
                    name = "uuid_gen_strategy_class",
                    value = "org.hibernate.id.uuid.CustomVersionOneStrategy"
                )
            }
        )
        public UUID id;

        ...

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">