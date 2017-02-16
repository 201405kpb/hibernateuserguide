### 29.2. JPA vs Hibernate entity name

    <div class="paragraph">

    When using the [`Session#createCriteria(String entityName)` or `StatelessSession#createCriteria(String entityName)`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/SharedSessionContract.html#createCriteria-java.lang.String-),
    the **entityName** means the fully-qualified name of the underlying entity and not the name denoted by the `name` attribute of the JPA `@Entity` annotation.

    </div>
    <div class="paragraph">

    Considering you have the following entity:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "ApplicationEvent")
    public static class Event {

        @Id
        private Long id;

        private String name;
    }`</pre>
    </div>
    </div>
    <div class="paragraph">

    If you provide the JPA entity name to a legacy Criteria query:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Event&gt; events =
        entityManager.unwrap( Session.class )
    .createCriteria( "ApplicationEvent" )
    .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    Hibernate is going to throw the following `MappingException`:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`org.hibernate.MappingException: Unknown entity: ApplicationEvent`</pre>
    </div>
    </div>
    <div class="paragraph">

    On the other hand, the Hibernate entity name (the fully qualified class name) works just fine:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Event&gt; events =
        entityManager.unwrap( Session.class )
    .createCriteria( Event.class.getName() )
    .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    For more about this topic, check out the [HHH-2597](https://hibernate.atlassian.net/browse/HHH-2597) JIRA issue.

    </div>
    </div>
    <div class="sect2">