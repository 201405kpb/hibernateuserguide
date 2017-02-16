### 14.4. Hibernate declarative security

    <div class="paragraph">

    Usually, declarative security in Hibernate applications is managed in a session facade layer.
    Hibernate allows certain actions to be authorized via JACC and JAAS.
    This is an optional functionality that is built on top of the event architecture.

    </div>
    <div class="paragraph">

    First, you must configure the appropriate event listeners, to enable the use of JACC authorization.
    Again, see [Event Listener Registration](#bootstrap-event-listener-registration) for the details.

    </div>
    <div class="paragraph">

    Below is an example of an appropriate `org.hibernate.integrator.spi.Integrator` implementation for this purpose.

    </div>
    <div id="events-declarative-security-jacc-example" class="exampleblock">
    <div class="title">Example 342. JACC listener registration example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public static class JaccIntegrator implements ServiceContributingIntegrator {

        private static final Logger log = Logger.getLogger( JaccIntegrator.class );

        private static final DuplicationStrategy DUPLICATION_STRATEGY =
                new DuplicationStrategy() {
            @Override
            public boolean areMatch(Object listener, Object original) {
                return listener.getClass().equals( original.getClass() ) &amp;&amp;
                        JaccSecurityListener.class.isInstance( original );
            }

            @Override
            public Action getAction() {
                return Action.KEEP_ORIGINAL;
            }
        };

        @Override
        public void prepareServices(
                StandardServiceRegistryBuilder serviceRegistryBuilder) {
            boolean isSecurityEnabled = serviceRegistryBuilder
                    .getSettings().containsKey( AvailableSettings.JACC_ENABLED );
            final JaccService jaccService = isSecurityEnabled ?
                    new StandardJaccServiceImpl() : new DisabledJaccServiceImpl();
            serviceRegistryBuilder.addService( JaccService.class, jaccService );
        }

        @Override
        public void integrate(
                Metadata metadata,
                SessionFactoryImplementor sessionFactory,
                SessionFactoryServiceRegistry serviceRegistry) {
            doIntegration(
                    serviceRegistry
                            .getService( ConfigurationService.class ).getSettings(),
                    // pass no permissions here, because atm actually injecting the
                    // permissions into the JaccService is handled on SessionFactoryImpl via
                    // the org.hibernate.boot.cfgxml.spi.CfgXmlAccessService
                    null,
                    serviceRegistry
            );
        }

        private void doIntegration(
                Map properties,
                JaccPermissionDeclarations permissionDeclarations,
                SessionFactoryServiceRegistry serviceRegistry) {
            boolean isSecurityEnabled = properties
                    .containsKey( AvailableSettings.JACC_ENABLED );
            if ( ! isSecurityEnabled ) {
                log.debug( "Skipping JACC integration as it was not enabled" );
                return;
            }

            final String contextId = (String) properties
                    .get( AvailableSettings.JACC_CONTEXT_ID );
            if ( contextId == null ) {
                throw new IntegrationException( "JACC context id must be specified" );
            }

            final JaccService jaccService = serviceRegistry
                    .getService( JaccService.class );
            if ( jaccService == null ) {
                throw new IntegrationException( "JaccService was not set up" );
            }

            if ( permissionDeclarations != null ) {
                for ( GrantedPermission declaration : permissionDeclarations
                        .getPermissionDeclarations() ) {
                    jaccService.addPermission( declaration );
                }
            }

            final EventListenerRegistry eventListenerRegistry =
                    serviceRegistry.getService( EventListenerRegistry.class );
            eventListenerRegistry.addDuplicationStrategy( DUPLICATION_STRATEGY );

            eventListenerRegistry.prependListeners(
                    EventType.PRE_DELETE, new JaccPreDeleteEventListener() );
            eventListenerRegistry.prependListeners(
                    EventType.PRE_INSERT, new JaccPreInsertEventListener() );
            eventListenerRegistry.prependListeners(
                    EventType.PRE_UPDATE, new JaccPreUpdateEventListener() );
            eventListenerRegistry.prependListeners(
                    EventType.PRE_LOAD, new JaccPreLoadEventListener() );
        }

        @Override
        public void disintegrate(SessionFactoryImplementor sessionFactory,
                                 SessionFactoryServiceRegistry serviceRegistry) {
            // nothing to do
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    You must also decide how to configure your JACC provider. Consult your JACC provider documentation.

    </div>
    </div>
    <div class="sect2">