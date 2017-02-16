#### 3.1.1. JPA-compliant bootstrapping

    <div class="paragraph">

    In JPA, we are ultimately interested in bootstrapping a `javax.persistence.EntityManagerFactory` instance.
    The JPA specification defines two primary standardized bootstrap approaches depending on how the application intends to access the `javax.persistence.EntityManager` instances from an `EntityManagerFactory`.

    </div>
    <div class="paragraph">

    It uses the terms _EE_ and _SE_ for these two approaches, but those terms are very misleading in this context.
    What the JPA spec calls EE bootstrapping implies the existence of a container (EE, OSGi, etc), who&#8217;ll manage and inject the persistence context on behalf of the application.
    What it calls SE bootstrapping is everything else. We will use the terms container-bootstrapping and application-bootstrapping in this guide.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    If you would like additional details on accessing and using `EntityManager` instances, sections 7.6 and 7.7 of the JPA 2.1 specification cover container-managed and application-managed `EntityManagers`, respectively.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    For compliant container-bootstrapping, the container will build an `EntityManagerFactory` for each persistent-unit defined in the `META-INF/persistence.xml` configuration file
    and make that available to the application for injection via the `javax.persistence.PersistenceUnit` annotation or via JNDI lookup.

    </div>
    <div id="bootstrap-jpa-compliant-PersistenceUnit-example" class="exampleblock">
    <div class="title">Example 200. Injecting a EntityManagerFactory</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@PersistenceUnit
    private EntityManagerFactory emf;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `META-INF/persistence.xml` file looks as follows:

    </div>
    <div id="bootstrap-jpa-compliant-persistence-xml-example" class="exampleblock">
    <div class="title">Example 201. META-INF/persistence.xml configuration file</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence
                 http://xmlns.jcp.org/xml/ns/persistence/persistence_2_1.xsd"
                 version="2.1"&gt;

        &lt;persistence-unit name="CRM"&gt;
            &lt;description&gt;
                Persistence unit for Hibernate User Guide
            &lt;/description&gt;

            &lt;provider&gt;org.hibernate.jpa.HibernatePersistenceProvider&lt;/provider&gt;

            &lt;class&gt;org.hibernate.documentation.userguide.Document&lt;/class&gt;

            &lt;properties&gt;
                &lt;property name="javax.persistence.jdbc.driver"
                          value="org.h2.Driver" /&gt;

                &lt;property name="javax.persistence.jdbc.url"
                          value="jdbc:h2:mem:db1;DB_CLOSE_DELAY=-1;MVCC=TRUE" /&gt;

                &lt;property name="javax.persistence.jdbc.user"
                          value="sa" /&gt;

                &lt;property name="javax.persistence.jdbc.password"
                          value="" /&gt;

                &lt;property name="hibernate.show_sql"
                          value="true" /&gt;

                &lt;property name="hibernate.hbm2ddl.auto"
                          value="update" /&gt;
            &lt;/properties&gt;

        &lt;/persistence-unit&gt;

    &lt;/persistence&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    For compliant application-bootstrapping, rather than the container building the `EntityManagerFactory` for the application, the application builds the `EntityManagerFactory` itself using the `javax.persistence.Persistence` bootstrap class.
    The application creates an `EntityManagerFactory` by calling the `createEntityManagerFactory` method:

    </div>
    <div id="bootstrap-jpa-compliant-EntityManagerFactory-example" class="exampleblock">
    <div class="title">Example 202. Application bootstrapped EntityManagerFactory</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`// Create an EMF for our CRM persistence-unit.
    EntityManagerFactory emf = Persistence.createEntityManagerFactory( "CRM" );`</pre>
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

    If you don&#8217;t want to provide a `persistence.xml` configuration file, JPA allows you to provide all the configuration options in a
    [PersistenceUnitInfo](http://docs.oracle.com/javaee/7/api/javax/persistence/spi/PersistenceUnitInfo.html) implementation and call
    [HibernatePersistenceProvider.html#createContainerEntityManagerFactory](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/jpa/HibernatePersistenceProvider.html#createContainerEntityManagerFactory-javax.persistence.spi.PersistenceUnitInfo-java.util.Map-).

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">