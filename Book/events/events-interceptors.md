 ### 14.1. Interceptors

    <div class="paragraph">

    The `org.hibernate.Interceptor` interface provides callbacks from the session to the application,
    allowing the application to inspect and/or manipulate properties of a persistent object before it is saved, updated, deleted or loaded.

    </div>
    <div class="paragraph">

    One possible use for this is to track auditing information.
    The following example shows an `Interceptor` implementation that automatically logs when an entity is updated.

    </div>
    <div id="events-interceptors-example" class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public static class LoggingInterceptor extends EmptyInterceptor {
        @Override
        public boolean onFlushDirty(
            Object entity,
            Serializable id,
            Object[] currentState,
            Object[] previousState,
            String[] propertyNames,
            Type[] types) {
                LOGGER.debugv( "Entity {0}#{1} changed from {2} to {3}",
                    entity.getClass().getSimpleName(),
                    id,
                    Arrays.toString( previousState ),
                    Arrays.toString( currentState )
                );
                return super.onFlushDirty( entity, id, currentState,
                    previousState, propertyNames, types
            );
        }
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

    You can either implement `Interceptor` directly or extend `org.hibernate.EmptyInterceptor`.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    An Interceptor can be either `Session`-scoped or `SessionFactory`-scoped.

    </div>
    <div class="paragraph">

    A Session-scoped interceptor is specified when a session is opened.

    </div>
    <div id="events-interceptors-session-scope-example" class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SessionFactory sessionFactory = entityManagerFactory.unwrap( SessionFactory.class );
    Session session = sessionFactory
        .withOptions()
        .interceptor(new LoggingInterceptor() )
        .openSession();
    session.getTransaction().begin();

    Customer customer = session.get( Customer.class, customerId );
    customer.setName( "Mr. John Doe" );
    //Entity Customer#1 changed from [John Doe, 0] to [Mr. John Doe, 0]

    session.getTransaction().commit();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    A `SessionFactory`-scoped interceptor is registered with the `Configuration` object prior to building the `SessionFactory`.
    Unless a session is opened explicitly specifying the interceptor to use, the `SessionFactory`-scoped interceptor will be applied to all sessions opened from that `SessionFactory`.
    `SessionFactory`-scoped interceptors must be thread safe.
    Ensure that you do not store session-specific states since multiple sessions will use this interceptor potentially concurrently.

    </div>
    <div id="events-interceptors-session-factory-scope-example" class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SessionFactory sessionFactory = new MetadataSources( new StandardServiceRegistryBuilder().build() )
        .addAnnotatedClass( Customer.class )
        .getMetadataBuilder()
        .build()
        .getSessionFactoryBuilder()
        .applyInterceptor( new LoggingInterceptor() )
        .build();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">