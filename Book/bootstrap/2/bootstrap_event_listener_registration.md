
    #### 3.2.2. Event Listener registration

    <div class="paragraph">

    The main use cases for an `org.hibernate.integrator.spi.Integrator` right now are registering event listeners and providing services (see `org.hibernate.integrator.spi.ServiceContributingIntegrator`).
    With 5.0 we plan on expanding that to allow altering the metamodel describing the mapping between object and relational models.

    </div>
    <div id="bootstrap-event-listener-registration-example" class="exampleblock">
    <div class="title">Example 207. Configuring an event listener</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public class MyIntegrator implements org.hibernate.integrator.spi.Integrator {

        @Override
        public void integrate(
                Metadata metadata,
                SessionFactoryImplementor sessionFactory,
                SessionFactoryServiceRegistry serviceRegistry) {

            // As you might expect, an EventListenerRegistry is the thing with which event
            // listeners are registered
            // It is a service so we look it up using the service registry
            final EventListenerRegistry eventListenerRegistry =
                serviceRegistry.getService( EventListenerRegistry.class );

            // If you wish to have custom determination and handling of "duplicate" listeners,
            // you would have to add an implementation of the
            // org.hibernate.event.service.spi.DuplicationStrategy contract like this
            eventListenerRegistry.addDuplicationStrategy( new CustomDuplicationStrategy() );

            // EventListenerRegistry defines 3 ways to register listeners:

            // 1) This form overrides any existing registrations with
            eventListenerRegistry.setListeners( EventType.AUTO_FLUSH,
                                                DefaultAutoFlushEventListener.class );

            // 2) This form adds the specified listener(s) to the beginning of the listener chain
            eventListenerRegistry.prependListeners( EventType.PERSIST,
                                                    DefaultPersistEventListener.class );

            // 3) This form adds the specified listener(s) to the end of the listener chain
            eventListenerRegistry.appendListeners( EventType.MERGE,
                                                   DefaultMergeEventListener.class );
        }

        @Override
        public void disintegrate(
                SessionFactoryImplementor sessionFactory,
                SessionFactoryServiceRegistry serviceRegistry) {

        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">