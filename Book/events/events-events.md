 ### 14.2. Native Event system

    <div class="paragraph">

    If you have to react to particular events in the persistence layer, you can also use the Hibernate _event_ architecture.
    The event system can be used in place of or in addition to interceptors.

    </div>
    <div class="paragraph">

    Many methods of the `Session` interface correlate to an event type.
    The full range of defined event types is declared as enum values on `org.hibernate.event.spi.EventType`.
    When a request is made of one of these methods, the Session generates an appropriate event and passes it to the configured event listener(s) for that type.

    </div>
    <div class="paragraph">

    Applications are free to implement a customization of one of the listener interfaces (i.e., the `LoadEvent` is processed by the registered implementation of the `LoadEventListener` interface), in which case their implementation would
    be responsible for processing any `load()` requests made of the `Session`.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The listeners should be considered stateless; they are shared between requests, and should not save any state as instance variables.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    A custom listener implements the appropriate interface for the event it wants to process and/or extend one of the convenience base classes
    (or even the default event listeners used by Hibernate out-of-the-box as these are declared non-final for this purpose).

    </div>
    <div class="paragraph">

    Here is an example of a custom load event listener:

    </div>
    <div id="events-interceptors-load-listener-example" class="exampleblock">
    <div class="title">Example 341. Custom `LoadListener` example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`EntityManagerFactory entityManagerFactory = entityManagerFactory();
    SessionFactoryImplementor sessionFactory = entityManagerFactory.unwrap( SessionFactoryImplementor.class );
    sessionFactory
        .getServiceRegistry()
        .getService( EventListenerRegistry.class )
        .prependListeners( EventType.LOAD, new SecuredLoadEntityListener() );

    Customer customer = entityManager.find( Customer.class, customerId );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">
