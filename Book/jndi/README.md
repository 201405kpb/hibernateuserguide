 ## 9. JNDI

    <div class="sectionbody">
    <div class="paragraph">

    Hibernate does optionally interact with JNDI on the application&#8217;s behalf.
    Generally, it does this when the application:

    </div>
    <div class="ulist">

*   has asked the SessionFactory be bound to JNDI
*   has specified a DataSource to use by JNDI name
*   is using JTA transactions and the `JtaPlatform` needs to do JNDI lookups for `TransactionManager`, `UserTransaction`, etc
    </div>
    <div class="paragraph">

    All of these JNDI calls route through a single service whose role is `org.hibernate.engine.jndi.spi.JndiService`.
    The standard `JndiService` accepts a number of configuration settings

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`hibernate.jndi.class`</dt>
    <dd>

    names the javax.naming.InitialContext implementation class to use. See [`javax.naming.Context#INITIAL_CONTEXT_FACTORY`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#INITIAL_CONTEXT_FACTORY)

    </dd>
    <dt class="hdlist1">`hibernate.jndi.url`</dt>
    <dd>

    names the JNDI InitialContext connection url. See [`javax.naming.Context.PROVIDER_URL`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#PROVIDER_URL)

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    Any other settings prefixed with `hibernate.jndi.` will be collected and passed along to the JNDI provider.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The standard `JndiService` assumes that all JNDI calls are relative to the same `InitialContext`.
    If your application uses multiple naming servers for whatever reason, you will need a custom `JndiService` implementation to handle those details.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    <div class="sect1">