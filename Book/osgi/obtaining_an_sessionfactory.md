 ### 20.17. Obtaining an SessionFactory

    <div class="paragraph">

    `hibernate-osgi` registers an OSGi service, using the `SessionFactory` interface name, that bootstraps and creates a `SessionFactory` specific for OSGi environments.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    It is VITAL that your `SessionFactory` be obtained through the service, rather than creating it manually. The service handles the OSGi `ClassLoader`, discovered extension points, scanning, etc.
    Manually creating a `SessionFactory` is guaranteed to NOT work during runtime!

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="exampleblock">
    <div class="title">Example 494. Discover/Use `SessionFactory`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public class HibernateUtil {

        private SessionFactory sf;

        public Session getSession() {
            return getSessionFactory().openSession();
        }

        private SessionFactory getSessionFactory() {
            if ( sf == null ) {
                Bundle thisBundle = FrameworkUtil.getBundle( HibernateUtil.class );
                BundleContext context = thisBundle.getBundleContext();

                ServiceReference sr = context.getServiceReference( SessionFactory.class.getName() );
                sf = ( SessionFactory ) context.getService( sr );
            }
            return sf;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">
