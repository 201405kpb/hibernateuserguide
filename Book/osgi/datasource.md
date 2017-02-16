### 20.8. DataSource

    <div class="paragraph">

    Typical Enterprise OSGi JPA usage includes a `DataSource` installed in the container.
    Your bundle&#8217;s `persistence.xml` calls out the `DataSource` through JNDI.
    For example, you could install the following H2 `DataSource`.
    You can deploy the `DataSource` manually (Karaf has a `deploy` dir), or through a "blueprint bundle" (`blueprint:file:/[PATH]/datasource-h2.xml`).

    </div>
    <div class="exampleblock">
    <div class="title">Example 490. datasource-h2.xml</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;?xml version="1.0" encoding="UTF-8"?&gt;
    &lt;!--
    First install the H2 driver using:
    &gt; install -s mvn:com.h2database/h2/1.3.163

    Then copy this file to the deploy folder
    --&gt;
    &lt;blueprint xmlns="http://www.osgi.org/xmlns/blueprint/v1.0.0"&gt;

        &lt;bean id="dataSource" class="org.h2.jdbcx.JdbcDataSource"&gt;
            &lt;property name="URL" value="jdbc:h2:mem:db1;DB_CLOSE_DELAY=-1;MVCC=TRUE"/&gt;
            &lt;property name="user" value="sa"/&gt;
            &lt;property name="password" value=""/&gt;
        &lt;/bean&gt;

        &lt;service interface="javax.sql.DataSource" ref="dataSource"&gt;
            &lt;service-properties&gt;
                &lt;entry key="osgi.jndi.service.name" value="jdbc/h2ds"/&gt;
            &lt;/service-properties&gt;
        &lt;/service&gt;
    &lt;/blueprint&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    That `DataSource` is then used by your `persistence.xml` persistence-unit. The following works in Karaf, but the names may need tweaked in alternative containers.

    </div>
    <div class="exampleblock">
    <div class="title">Example 491. META-INF/persistence.xml</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;jta-data-source&gt;osgi:service/javax.sql.DataSource/(osgi.jndi.service.name=jdbc/h2ds)&lt;/jta-data-source&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">