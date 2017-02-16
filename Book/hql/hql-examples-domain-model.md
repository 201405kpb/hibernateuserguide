### 15.2. Examples domain model

    <div class="paragraph">

    To better understand the further HQL and JPQL examples, it&#8217;s time to familiarize the domain model entities that are used in all the examples features in this chapter.

    </div>
    <div id="hql-examples-domain-model-example" class="exampleblock">
    <div class="title">Example 344. Examples domain model</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@NamedQueries(
        @NamedQuery(
            name = "get_person_by_name",
            query = "select p from Person p where name = :name"
        )
    )
    @Entity
    public class Person {

        @Id
        @GeneratedValue
        private Long id;

        private String name;

        private String nickName;

        private String address;

        @Temporal(TemporalType.TIMESTAMP )
        private Date createdOn;

        @OneToMany(mappedBy = "person", cascade = CascadeType.ALL)
        @OrderColumn(name = "order_id")
        private List&lt;Phone&gt; phones = new ArrayList&lt;&gt;();

        @ElementCollection
        @MapKeyEnumerated(EnumType.STRING)
        private Map&lt;AddressType, String&gt; addresses = new HashMap&lt;&gt;();

        @Version
        private int version;

        //Getters and setters are omitted for brevity

    }

    public enum AddressType {
        HOME,
        OFFICE
    }

    @Entity
    public class Partner {

    	@Id
    	@GeneratedValue
    	private Long id;

    	private String name;

    	@Version
    	private int version;

    	//Getters and setters are omitted for brevity

    }

    @Entity
    public class Phone {

        @Id
        private Long id;

        @ManyToOne(fetch = FetchType.LAZY)
        private Person person;

        @Column(name = "phone_number")
        private String number;

        @Enumerated(EnumType.STRING)
        @Column(name = "phone_type")
        private PhoneType type;

        @OneToMany(mappedBy = "phone", cascade = CascadeType.ALL, orphanRemoval = true)
        private List&lt;Call&gt; calls = new ArrayList&lt;&gt;(  );

        @OneToMany(mappedBy = "phone")
        @MapKey(name = "timestamp")
        @MapKeyTemporal(TemporalType.TIMESTAMP )
        private Map&lt;Date, Call&gt; callHistory = new HashMap&lt;&gt;();

        @ElementCollection
        private List&lt;Date&gt; repairTimestamps = new ArrayList&lt;&gt;(  );

        //Getters and setters are omitted for brevity

    }

    public enum PhoneType {
        LAND_LINE,
        MOBILE;
    }

    @Entity
    @Table(name = "phone_call")
    public class Call {

        @Id
        @GeneratedValue
        private Long id;

        @ManyToOne
        private Phone phone;

        @Column(name = "call_timestamp")
        private Date timestamp;

        private int duration;

        //Getters and setters are omitted for brevity

    }

    @Entity
    @Inheritance(strategy = InheritanceType.JOINED)
    public class Payment {

        @Id
        @GeneratedValue
        private Long id;

        private BigDecimal amount;

        private boolean completed;

        @ManyToOne
        private Person person;

        //Getters and setters are omitted for brevity

    }

    @Entity
    public class CreditCardPayment extends Payment {
    }

    @Entity
    public class WireTransferPayment extends Payment {
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">