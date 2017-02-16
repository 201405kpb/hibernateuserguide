### 20.19. Extension Points

    <div class="paragraph">

    Multiple contracts exist to allow applications to integrate with and extend Hibernate capabilities.
    Most apps utilize JDK services to provide their implementations.
    `hibernate-osgi` supports the same extensions through OSGi services.
    Implement and register them in any of the three configurations.
    `hibernate-osgi` will discover and integrate them during `EntityManagerFactory`/`SessionFactory` bootstrapping. Supported extension points are as follows.
    The specified interface should be used during service registration.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`org.hibernate.integrator.spi.Integrator`</dt>
    <dd>

    (as of 4.2)

    </dd>
    <dt class="hdlist1">`org.hibernate.boot.registry.selector.StrategyRegistrationProvider`</dt>
    <dd>

    (as of 4.3)

    </dd>
    <dt class="hdlist1">`org.hibernate.boot.model.TypeContributor`</dt>
    <dd>

    (as of 4.3)

    </dd>
    <dt class="hdlist1">JTA&#8217;s</dt>
    <dd>

    `javax.transaction.TransactionManager` and `javax.transaction.UserTransaction` (as of 4.2), however these are typically provided by the OSGi container.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    The easiest way to register extension point implementations is through a `blueprint.xml` file.
    Add `OSGI-INF/blueprint/blueprint.xml` to your classpath. Envers' blueprint is a great example:

    </div>
    <div class="exampleblock">
    <div class="title">Example 495. Example extension point registrations in blueprint.xml</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;!--
      ~ Hibernate, Relational Persistence for Idiomatic Java
      ~
      ~ License: GNU Lesser General Public License (LGPL), version 2.1 or later.
      ~ See the lgpl.txt file in the root directory or &lt;http://www.gnu.org/licenses/lgpl-2.1.html&gt;.
      --&gt;
    &lt;blueprint default-activation="eager"
               xmlns="http://www.osgi.org/xmlns/blueprint/v1.0.0"&gt;

        &lt;bean id="integrator" class="org.hibernate.envers.boot.internal.EnversIntegrator"/&gt;
        &lt;service ref="integrator" interface="org.hibernate.integrator.spi.Integrator"/&gt;

        &lt;bean id="typeContributor"
              class="org.hibernate.envers.boot.internal.TypeContributorImpl"/&gt;
        &lt;service ref="typeContributor" interface="org.hibernate.boot.model.TypeContributor"/&gt;

    &lt;/blueprint&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Extension points can also be registered programmatically with `BundleContext#registerService`, typically within your `BundleActivator#start`.

    </div>
    </div>
    <div class="sect2">