  #### 3.2.4. Building the SessionFactory

    <div class="paragraph">

    The final step in native bootstrapping is to build the `SessionFactory` itself.
    Much like discussed above, if you are ok with the default behavior of building a `SessionFactory` from a `Metadata` reference, you can simply call the [`buildSessionFactory`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/Metadata.html#buildSessionFactory--) method on the `Metadata` object.

    </div>
    <div class="paragraph">

    However, if you would like to adjust that building process you will need to use `SessionFactoryBuilder` as obtained via [`Metadata#getSessionFactoryBuilder`. Again, see its [Javadocs](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/Metadata.html#getSessionFactoryBuilder--) for more details.

    </div>
    <div id="bootstrap-native-SessionFactory-example" class="exampleblock">
    <div class="title">Example 210. Native Bootstrapping - Putting it all together</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`StandardServiceRegistry standardRegistry = new StandardServiceRegistryBuilder()
        .configure( "org/hibernate/example/hibernate.cfg.xml" )
        .build();

    Metadata metadata = new MetadataSources( standardRegistry )
        .addAnnotatedClass( MyEntity.class )
        .addAnnotatedClassName( "org.hibernate.example.Customer" )
        .addResource( "org/hibernate/example/Order.hbm.xml" )
        .addResource( "org/hibernate/example/Product.orm.xml" )
        .getMetadataBuilder()
        .applyImplicitNamingStrategy( ImplicitNamingStrategyJpaCompliantImpl.INSTANCE )
        .build();

    SessionFactory sessionFactory = metadata.getSessionFactoryBuilder()
        .applyBeanManager( getBeanManager() )
        .build();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The bootstrapping API is quite flexible, but in most cases it makes the most sense to think of it as a 3 step process:

    </div>
    <div class="olist arabic">

1.  Build the `StandardServiceRegistry`
2.  Build the `Metadata`
3.  Use those 2 to build the `SessionFactory`
    </div>
    <div id="bootstrap-native-SessionFactoryBuilder-example" class="exampleblock">
    <div class="title">Example 211. Building `SessionFactory` via `SessionFactoryBuilder`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`StandardServiceRegistry standardRegistry = new StandardServiceRegistryBuilder()
            .configure( "org/hibernate/example/hibernate.cfg.xml" )
            .build();

    Metadata metadata = new MetadataSources( standardRegistry )
        .addAnnotatedClass( MyEntity.class )
        .addAnnotatedClassName( "org.hibernate.example.Customer" )
        .addResource( "org/hibernate/example/Order.hbm.xml" )
        .addResource( "org/hibernate/example/Product.orm.xml" )
        .getMetadataBuilder()
        .applyImplicitNamingStrategy( ImplicitNamingStrategyJpaCompliantImpl.INSTANCE )
        .build();

    SessionFactoryBuilder sessionFactoryBuilder = metadata.getSessionFactoryBuilder();

    // Supply an SessionFactory-level Interceptor
    sessionFactoryBuilder.applyInterceptor( new CustomSessionFactoryInterceptor() );

    // Add a custom observer
    sessionFactoryBuilder.addSessionFactoryObservers( new CustomSessionFactoryObserver() );

    // Apply a CDI BeanManager ( for JPA event listeners )
    sessionFactoryBuilder.applyBeanManager( getBeanManager() );

    SessionFactory sessionFactory = sessionFactoryBuilder.build();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">
