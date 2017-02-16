 ### 11.5. Dynamic fetching via JPA entity graph

    <div class="paragraph">

    JPA 2.1 introduced entity graphs so the application developer has more control over fetch plans.

    </div>
    <div id="fetching-strategies-dynamic-fetching-entity-graph-example" class="exampleblock">
    <div class="title">Example 288. Fetch graph example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Employee")
    @NamedEntityGraph(name = "employee.projects",
        attributeNodes = @NamedAttributeNode("projects")
    )`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Employee employee = entityManager.find(
        Employee.class,
        userId,
        Collections.singletonMap(
            "javax.persistence.fetchgraph",
            entityManager.getEntityGraph( "employee.projects" )
        )
    );`</pre>
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

    Entity graphs are the way to override the EAGER fetching associations at runtime.
    With JPQL, if an EAGER association is omitted, Hibernate will issue a secondary select for every association needed to be fetched eagerly.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">