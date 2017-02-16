#### 19.4.1. MultiTenantConnectionProvider

    <div class="paragraph">

    When using either the DATABASE or SCHEMA approach, Hibernate needs to be able to obtain Connections in a tenant-specific manner.

    </div>
    <div class="paragraph">

    That is the role of the `MultiTenantConnectionProvider` contract.
    Application developers will need to provide an implementation of this contract.

    </div>
    <div class="paragraph">

    Most of its methods are extremely self-explanatory.
    The only ones which might not be are `getAnyConnection` and `releaseAnyConnection`.
    It is important to note also that these methods do not accept the tenant identifier.
    Hibernate uses these methods during startup to perform various configuration, mainly via the `java.sql.DatabaseMetaData` object.

    </div>
    <div class="paragraph">

    The `MultiTenantConnectionProvider` to use can be specified in a number of ways:

    </div>
    <div class="ulist">

*   Use the `hibernate.multi_tenant_connection_provider` setting.
    It could name a `MultiTenantConnectionProvider` instance, a `MultiTenantConnectionProvider` implementation class reference or a `MultiTenantConnectionProvider` implementation class name.
*   Passed directly to the `org.hibernate.boot.registry.StandardServiceRegistryBuilder`.
*   If none of the above options match, but the settings do specify a `hibernate.connection.datasource` value,
    Hibernate will assume it should use the specific `DataSourceBasedMultiTenantConnectionProviderImpl` implementation which works on a number of pretty reasonable assumptions when running inside of an app server and using one `javax.sql.DataSource` per tenant.
    See its [Javadocs](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/jdbc/connections/spi/DataSourceBasedMultiTenantConnectionProviderImpl.html) for more details.
    </div>
    <div class="paragraph">

    The following example portrays a `MultiTenantConnectionProvider` implementation that handles multiple `ConnectionProviders`.

    </div>
    <div id="multitenacy-hibernate-ConfigurableMultiTenantConnectionProvider-example" class="exampleblock">
    <div class="title">Example 487. A `MultiTenantConnectionProvider` implementation</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public class ConfigurableMultiTenantConnectionProvider
            extends AbstractMultiTenantConnectionProvider {

        private final Map&lt;String, ConnectionProvider&gt; connectionProviderMap =
            new HashMap&lt;&gt;(  );

        public ConfigurableMultiTenantConnectionProvider(
                Map&lt;String, ConnectionProvider&gt; connectionProviderMap) {
            this.connectionProviderMap.putAll( connectionProviderMap );
        }

        @Override
        protected ConnectionProvider getAnyConnectionProvider() {
            return connectionProviderMap.values().iterator().next();
        }

        @Override
        protected ConnectionProvider selectConnectionProvider(String tenantIdentifier) {
            return connectionProviderMap.get( tenantIdentifier );
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `ConfigurableMultiTenantConnectionProvider` can be set up as follows:

    </div>
    <div id="multitenacy-hibernate-MultiTenantConnectionProvider-example" class="exampleblock">
    <div class="title">Example 488. A `MultiTenantConnectionProvider` implementation</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`private void init() {
        registerConnectionProvider( FRONT_END_TENANT );
        registerConnectionProvider( BACK_END_TENANT );

        Map&lt;String, Object&gt; settings = new HashMap&lt;&gt;(  );

        settings.put( AvailableSettings.MULTI_TENANT, multiTenancyStrategy() );
        settings.put( AvailableSettings.MULTI_TENANT_CONNECTION_PROVIDER,
            new ConfigurableMultiTenantConnectionProvider( connectionProviderMap ) );

        sessionFactory = sessionFactory(settings);
    }

    protected void registerConnectionProvider(String tenantIdentifier) {
        Properties properties = properties();
        properties.put( Environment.URL,
            tenantUrl(properties.getProperty( Environment.URL ), tenantIdentifier) );

        DriverManagerConnectionProviderImpl connectionProvider =
            new DriverManagerConnectionProviderImpl();
        connectionProvider.configure( properties );
        connectionProviderMap.put( tenantIdentifier, connectionProvider );
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When using multitenancy, it&#8217;s possible to save an entity with the same identifier across different tenants:

    </div>
    <div id="multitenacy-hibernate-same-entity-example" class="exampleblock">
    <div class="title">Example 489. A `MultiTenantConnectionProvider` implementation</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInSession( FRONT_END_TENANT, session -&gt; {
        Person person = new Person(  );
        person.setId( 1L );
        person.setName( "John Doe" );
        session.persist( person );
    } );

    doInSession( BACK_END_TENANT, session -&gt; {
        Person person = new Person(  );
        person.setId( 1L );
        person.setName( "John Doe" );
        session.persist( person );
    } );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">