 #### 3.2.3. Building the Metadata

    <div class="paragraph">

    The second step in native bootstrapping is the building of a `org.hibernate.boot.Metadata` object containing the parsed representations of an application domain model and its mapping to a database.
    The first thing we obviously need to build a parsed representation is the source information to be parsed (annotated classes, `hbm.xml` files, `orm.xml` files).
    This is the purpose of `org.hibernate.boot.MetadataSources`:

    </div>
    <div class="paragraph">

    `MetadataSources` has many other methods as well, explore its API and [Javadocs](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/MetadataSources.html) for more information.
    Also, all methods on `MetadataSources` offer fluent-style call chaining::

    </div>
    <div id="bootstrap-native-metadata-source-example" class="exampleblock">
    <div class="title">Example 208. Configuring a `MetadataSources` with method chaining</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`ServiceRegistry standardRegistry =
            new StandardServiceRegistryBuilder().build();

    MetadataSources sources = new MetadataSources( standardRegistry )
        .addAnnotatedClass( MyEntity.class )
        .addAnnotatedClassName( "org.hibernate.example.Customer" )
        .addResource( "org/hibernate/example/Order.hbm.xml" )
        .addResource( "org/hibernate/example/Product.orm.xml" );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Once we have the sources of mapping information defined, we need to build the `Metadata` object.
    If you are ok with the default behavior in building the Metadata then you can simply call the [`buildMetadata`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/MetadataSources.html#buildMetadata--) method of the `MetadataSources`.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Notice that a `ServiceRegistry` can be passed at a number of points in this bootstrapping process.
    The suggested approach is to build a `StandardServiceRegistry` yourself and pass that along to the `MetadataSources` constructor.
    From there, `MetadataBuilder`, `Metadata`, `SessionFactoryBuilder` and `SessionFactory` will all pick up that same `StandardServiceRegistry`.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    However, if you wish to adjust the process of building `Metadata` from `MetadataSources`,
    you will need to use the `MetadataBuilder` as obtained via `MetadataSources#getMetadataBuilder`.
    `MetadataBuilder` allows a lot of control over the `Metadata` building process.
    See its [Javadocs](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/MetadataBuilder.html) for full details.

    </div>
    <div id="bootstrap-native-metadata-builder-example" class="exampleblock">
    <div class="title">Example 209. Building Metadata via `MetadataBuilder`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`ServiceRegistry standardRegistry =
        new StandardServiceRegistryBuilder().build();

    MetadataSources sources = new MetadataSources( standardRegistry );

    MetadataBuilder metadataBuilder = sources.getMetadataBuilder();

    // Use the JPA-compliant implicit naming strategy
    metadataBuilder.applyImplicitNamingStrategy(
        ImplicitNamingStrategyJpaCompliantImpl.INSTANCE );

    // specify the schema name to use for tables, etc when none is explicitly specified
    metadataBuilder.applyImplicitSchemaName( "my_default_schema" );

    // specify a custom Attribute Converter
    metadataBuilder.applyAttributeConverter( myAttributeConverter );

    Metadata metadata = metadataBuilder.build();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">