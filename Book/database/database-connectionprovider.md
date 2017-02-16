  ### 7.1. ConnectionProvider

    <div class="paragraph">

    As an ORM tool, probably the single most important thing you need to tell Hibernate is how to connect to your database so that it may connect on behalf of your application.
    This is ultimately the function of the `org.hibernate.engine.jdbc.connections.spi.ConnectionProvider` interface.
    Hibernate provides some out of the box implementations of this interface.
    `ConnectionProvider` is also an extension point so you can also use custom implementations from third parties or written yourself.
    The `ConnectionProvider` to use is defined by the `hibernate.connection.provider_class` setting. See the [`org.hibernate.cfg.AvailableSettings#CONNECTION_PROVIDER`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/cfg/AvailableSettings.html#CONNECTION_PROVIDER)

    </div>
    <div class="paragraph">

    Generally speaking, applications should not have to configure a `ConnectionProvider` explicitly if using one of the Hibernate-provided implementations.
    Hibernate will internally determine which `ConnectionProvider` to use based on the following algorithm:

    </div>
    <div class="olist arabic">

1.  If `hibernate.connection.provider_class` is set, it takes precedence
2.  else if `hibernate.connection.datasource` is set &#8594; [Using DataSources](#database-connectionprovider-datasource)
3.  else if any setting prefixed by `hibernate.c3p0.` is set &#8594; [Using c3p0](#database-connectionprovider-c3p0)
4.  else if any setting prefixed by `hibernate.proxool.` is set &#8594; [Using Proxool](#database-connectionprovider-proxool)
5.  else if any setting prefixed by `hibernate.hikari.` is set &#8594; [Using Hikari](#database-connectionprovider-hikari)
6.  else if `hibernate.connection.url` is set &#8594; [Using Hibernate&#8217;s built-in (and unsupported) pooling](#database-connectionprovider-drivermanager)
7.  else &#8594; [User-provided Connections](#database-connectionprovider-provided)
    </div>
    </div>
    <div class="sect2">