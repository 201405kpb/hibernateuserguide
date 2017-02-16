### 11.2. Applying fetch strategies

    <div class="paragraph">

    Let&#8217;s consider these topics as it relates to an simple domain model and a few use cases.

    </div>
    <div id="fetching-strategies-domain-model-example" class="exampleblock">
    <div class="title">Example 283. Sample domain model</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Department")
    public static class Department {

        @Id
        private Long id;

        @OneToMany(mappedBy = "department")
        private List&lt;Employee&gt; employees = new ArrayList&lt;&gt;();

        //Getters and setters omitted for brevity
    }

    @Entity(name = "Employee")
    public static class Employee {

        @Id
        private Long id;

        @NaturalId
        private String username;

        @Column(name = "pswd")
        @ColumnTransformer(
            read = "decrypt( 'AES', '00', pswd  )",
            write = "encrypt('AES', '00', ?)"
        )
        private String password;

        private int accessLevel;

        @ManyToOne(fetch = FetchType.LAZY)
        private Department department;

        @ManyToMany(mappedBy = "employees")
        private List&lt;Project&gt; projects = new ArrayList&lt;&gt;();

        //Getters and setters omitted for brevity
    }

    @Entity(name = "Project")
    public class Project {

        @Id
        private Long id;

        @ManyToMany
        private List&lt;Employee&gt; employees = new ArrayList&lt;&gt;();

        //Getters and setters omitted for brevity
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The Hibernate recommendation is to statically mark all associations lazy and to use dynamic fetching strategies for eagerness.
    This is unfortunately at odds with the JPA specification which defines that all one-to-one and many-to-one associations should be eagerly fetched by default.
    Hibernate, as a JPA provider, honors that default.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">