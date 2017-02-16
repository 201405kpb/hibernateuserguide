### 23.13. Multi-tenancy settings

    <table class="tableblock frame-all grid-all spread">
    <colgroup>
    <col style="width: 20%;">
    <col style="width: 20%;">
    <col style="width: 60%;">
    </colgroup>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    Property
    </td>
    <td class="tableblock halign-left valign-top">

    Example
    </td>
    <td class="tableblock halign-left valign-top">

    Purpose
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.multiTenancy`
    </td>
    <td class="tableblock halign-left valign-top">

    `NONE` (default value), `SCHEMA`, `DATABASE`, and `DISCRIMINATOR` (not implemented yet)
    </td>
    <td class="tableblock halign-left valign-top">

    The multi-tenancy strategy in use.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.multi_tenant_connection_provider`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Names a [`MultiTenantConnectionProvider`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/jdbc/connections/spi/MultiTenantConnectionProvider.html) implementation to use. As `MultiTenantConnectionProvider` is also a service, can be configured directly through the [`StandardServiceRegistryBuilder`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/registry/StandardServiceRegistryBuilder.html).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.tenant_identifier_resolver`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Names a [`CurrentTenantIdentifierResolver`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/context/spi/CurrentTenantIdentifierResolver.html) implementation to resolve the resolve the current tenant identifier so that calling `SessionFactory#openSession()` would get a `Session` that&#8217;s connected to the right tenant.

    </div>
    <div class="paragraph">

    Can be:

    </div>
    <div class="ulist">

*   `CurrentTenantIdentifierResolver` instance
*   `CurrentTenantIdentifierResolver` implementation `Class` object reference
*   `CurrentTenantIdentifierResolver` implementation class name
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.multi_tenant.datasource.identifier_for_any`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    When the `hibernate.connection.datasource` property value is resolved to a `javax.naming.Context` object, this configuration property defines the JNDI name used to locate the `DataSource` used for fetching the initial `Connection` which is used to access to the database metadata of the underlying database(s) (in situations where we do not have a tenant id, like startup processing).
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">