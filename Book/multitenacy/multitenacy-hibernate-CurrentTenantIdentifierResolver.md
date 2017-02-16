 #### 19.4.2. CurrentTenantIdentifierResolver

    <div class="paragraph">

    `org.hibernate.context.spi.CurrentTenantIdentifierResolver` is a contract for Hibernate to be able to resolve what the application considers the current tenant identifier.
    The implementation to use is either passed directly to `Configuration` via its `setCurrentTenantIdentifierResolver` method.
    It can also be specified via the `hibernate.tenant_identifier_resolver` setting.

    </div>
    <div class="paragraph">

    There are two situations where CurrentTenantIdentifierResolver is used:

    </div>
    <div class="ulist">

*   The first situation is when the application is using the `org.hibernate.context.spi.CurrentSessionContext` feature in conjunction with multitenancy.
    In the case of the current-session feature, Hibernate will need to open a session if it cannot find an existing one in scope.
    However, when a session is opened in a multitenant environment, the tenant identifier has to be specified.
    This is where the `CurrentTenantIdentifierResolver` comes into play; Hibernate will consult the implementation you provide to determine the tenant identifier to use when opening the session.
    In this case, it is required that a `CurrentTenantIdentifierResolver` is supplied.
*   The other situation is when you do not want to have to explicitly specify the tenant identifier all the time.
    If a `CurrentTenantIdentifierResolver` has been specified, Hibernate will use it to determine the default tenant identifier to use when opening the session.
    </div>
    <div class="paragraph">

    Additionally, if the `CurrentTenantIdentifierResolver` implementation returns `true` for its `validateExistingCurrentSessions` method, Hibernate will make sure any existing sessions that are found in scope have a matching tenant identifier.
    This capability is only pertinent when the `CurrentTenantIdentifierResolver` is used in current-session settings.

    </div>
    </div>
    <div class="sect3">
