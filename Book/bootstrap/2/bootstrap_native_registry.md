 #### 3.2.1. Building the ServiceRegistry

    <div class="paragraph">

    The first step in native bootstrapping is the building of a `ServiceRegistry` holding the services Hibernate will need during bootstrapping and at run time.

    </div>
    <div class="paragraph">

    Actually, we are concerned with building 2 different ServiceRegistries.
    First is the `org.hibernate.boot.registry.BootstrapServiceRegistry`.
    The `BootstrapServiceRegistry` is intended to hold services that Hibernate needs at both bootstrap and run time.
    This boils down to 3 services:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`org.hibernate.boot.registry.classloading.spi.ClassLoaderService`</dt>
    <dd>

    which controls how Hibernate interacts with `ClassLoader`s

    </dd>
    <dt class="hdlist1">`org.hibernate.integrator.spi.IntegratorService`</dt>
    <dd>

    which controls the management and discovery of `org.hibernate.integrator.spi.Integrator` instances.

    </dd>
    <dt class="hdlist1">`org.hibernate.boot.registry.selector.spi.StrategySelector`</dt>
    <dd>

    which control how Hibernate resolves implementations of various strategy contracts.
    This is a very powerful service, but a full discussion of it is beyond the scope of this guide.

    </dd>
    </dl>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    If you are ok with the default behavior of Hibernate in regards to these `BootstrapServiceRegistry` services
    (which is quite often the case, especially in stand-alone environments), then building the `BootstrapServiceRegistry` can be skipped.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    If you wish to alter how the `BootstrapServiceRegistry` is built, that is controlled through the `org.hibernate.boot.registry.BootstrapServiceRegistryBuilder:`

    </div>
    <div id="bootstrap-bootstrap-native-registry-BootstrapServiceRegistry-example" class="exampleblock">
    <div class="title">Example 204. Controlling `BootstrapServiceRegistry` building</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`BootstrapServiceRegistryBuilder bootstrapRegistryBuilder =
        new BootstrapServiceRegistryBuilder();
    // add a custom ClassLoader
    bootstrapRegistryBuilder.applyClassLoader( customClassLoader );
    // manually add an Integrator
    bootstrapRegistryBuilder.applyIntegrator( customIntegrator );

    BootstrapServiceRegistry bootstrapRegistry = bootstrapRegistryBuilder.build();`</pre>
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

    The services of the `BootstrapServiceRegistry` cannot be extended (added to) nor overridden (replaced).

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The second ServiceRegistry is the `org.hibernate.boot.registry.StandardServiceRegistry`.
    You will almost always need to configure the `StandardServiceRegistry`, which is done through `org.hibernate.boot.registry.StandardServiceRegistryBuilder`:

    </div>
    <div id="bootstrap-bootstrap-native-registry-StandardServiceRegistryBuilder-example" class="exampleblock">
    <div class="title">Example 205. Building a `BootstrapServiceRegistryBuilder`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`// An example using an implicitly built BootstrapServiceRegistry
    StandardServiceRegistryBuilder standardRegistryBuilder =
        new StandardServiceRegistryBuilder();

    // An example using an explicitly built BootstrapServiceRegistry
    BootstrapServiceRegistry bootstrapRegistry =
        new BootstrapServiceRegistryBuilder().build();

    StandardServiceRegistryBuilder standardRegistryBuilder =
        new StandardServiceRegistryBuilder( bootstrapRegistry );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    A `StandardServiceRegistry` is also highly configurable via the StandardServiceRegistryBuilder API.
    See the `StandardServiceRegistryBuilder` [Javadocs](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/registry/StandardServiceRegistryBuilder.html) for more details.

    </div>
    <div class="paragraph">

    Some specific methods of interest:

    </div>
    <div id="bootstrap-bootstrap-native-registry-MetadataSources-example" class="exampleblock">
    <div class="title">Example 206. Configuring a `MetadataSources`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`ServiceRegistry standardRegistry =
            new StandardServiceRegistryBuilder().build();

    MetadataSources sources = new MetadataSources( standardRegistry );

    // alternatively, we can build the MetadataSources without passing
    // a service registry, in which case it will build a default
    // BootstrapServiceRegistry to use.  But the approach shown
    // above is preferred
    // MetadataSources sources = new MetadataSources();

    // add a class using JPA/Hibernate annotations for mapping
    sources.addAnnotatedClass( MyEntity.class );

    // add the name of a class using JPA/Hibernate annotations for mapping.
    // differs from above in that accessing the Class is deferred which is
    // important if using runtime bytecode-enhancement
    sources.addAnnotatedClassName( "org.hibernate.example.Customer" );

    // Read package-level metadata.
    sources.addPackage( "hibernate.example" );

    // Read package-level metadata.
    sources.addPackage( MyEntity.class.getPackage() );

    // Adds the named hbm.xml resource as a source: which performs the
    // classpath lookup and parses the XML
    sources.addResource( "org/hibernate/example/Order.hbm.xml" );

    // Adds the named JPA orm.xml resource as a source: which performs the
    // classpath lookup and parses the XML
    sources.addResource( "org/hibernate/example/Product.orm.xml" );

    // Read all mapping documents from a directory tree.
    // Assumes that any file named *.hbm.xml is a mapping document.
    sources.addDirectory( new File( ".") );

    // Read mappings from a particular XML file
    sources.addFile( new File( "./mapping.xml") );

    // Read all mappings from a jar file.
    // Assumes that any file named *.hbm.xml is a mapping document.
    sources.addJar( new File( "./entities.jar") );

    // Read a mapping as an application resource using the convention that a class named foo.bar.MyEntity is
    // mapped by a file named foo/bar/MyEntity.hbm.xml which can be resolved as a classpath resource.
    sources.addClass( MyEntity.class );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">