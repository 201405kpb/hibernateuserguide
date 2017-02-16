 ### 20.14. Obtaining an EntityMangerFactory

    <div class="paragraph">

    `hibernate-osgi` registers an OSGi service, using the JPA `PersistenceProvider` interface name, that bootstraps and creates an `EntityManagerFactory` specific for OSGi environments.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    It is VITAL that your `EntityManagerFactory` be obtained through the service, rather than creating it manually.
    The service handles the OSGi `ClassLoader`, discovered extension points, scanning, etc.
    Manually creating an `EntityManagerFactory` is guaranteed to NOT work during runtime!

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="exampleblock">
    <div class="title">Example 493. Discover/Use `EntityManagerFactory`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public class HibernateUtil {

        private EntityManagerFactory emf;

        public EntityManager getEntityManager() {
            return getEntityManagerFactory().createEntityManager();
        }

        private EntityManagerFactory getEntityManagerFactory() {
            if ( emf == null ) {
                Bundle thisBundle = FrameworkUtil.getBundle( HibernateUtil.class );
                BundleContext context = thisBundle.getBundleContext();

                ServiceReference serviceReference = context.getServiceReference( PersistenceProvider.class.getName() );
                PersistenceProvider persistenceProvider = ( PersistenceProvider ) context.getService( serviceReference );

                emf = persistenceProvider.createEntityManagerFactory( "YourPersistenceUnitName", null );
            }
            return emf;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">