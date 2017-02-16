 ### 8.2. JTA configuration

    <div class="paragraph">

    Interaction with a JTA system is consolidated behind a single contract named `org.hibernate.engine.transaction.jta.platform.spi.JtaPlatform` which exposes access to the `javax.transaction.TransactionManager`
    and `javax.transaction.UserTransaction` for that system as well as exposing the ability to register `javax.transaction.Synchronization` instances, check transaction status, etc.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Generally, `JtaPlatform` will need access to JNDI to resolve the JTA `TransactionManager`, `UserTransaction`, etc.
    See [JNDI chapter](#jndi) for details on configuring access to JNDI.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Hibernate tries to discover the `JtaPlatform` it should use through the use of another service named `org.hibernate.engine.transaction.jta.platform.spi.JtaPlatformResolver`.
    If that resolution does not work, or if you wish to provide a custom implementation you will need to specify the `hibernate.transaction.jta.platform` setting.
    Hibernate provides many implementations of the `JtaPlatform` contract, all with short names:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`Borland`</dt>
    <dd>

    `JtaPlatform` for the Borland Enterprise Server.

    </dd>
    <dt class="hdlist1">`Bitronix`</dt>
    <dd>

    `JtaPlatform` for Bitronix.

    </dd>
    <dt class="hdlist1">`JBossAS`</dt>
    <dd>

    `JtaPlatform` for Arjuna/JBossTransactions/Narayana when used within the JBoss/WildFly Application Server.

    </dd>
    <dt class="hdlist1">`JBossTS`</dt>
    <dd>

    `JtaPlatform` for Arjuna/JBossTransactions/Narayana when used standalone.

    </dd>
    <dt class="hdlist1">`JOnAS`</dt>
    <dd>

    `JtaPlatform` for JOTM when used within JOnAS.

    </dd>
    <dt class="hdlist1">`JOTM`</dt>
    <dd>

    `JtaPlatform` for JOTM when used standalone.

    </dd>
    <dt class="hdlist1">`JRun4`</dt>
    <dd>

    `JtaPlatform` for the JRun 4 Application Server.

    </dd>
    <dt class="hdlist1">`OC4J`</dt>
    <dd>

    `JtaPlatform` for Oracle&#8217;s OC4J container.

    </dd>
    <dt class="hdlist1">`Orion`</dt>
    <dd>

    `JtaPlatform` for the Orion Application Server.

    </dd>
    <dt class="hdlist1">`Resin`</dt>
    <dd>

    `JtaPlatform` for the Resin Application Server.

    </dd>
    <dt class="hdlist1">`SunOne`</dt>
    <dd>

    `JtaPlatform` for the SunOne Application Server.

    </dd>
    <dt class="hdlist1">`Weblogic`</dt>
    <dd>

    `JtaPlatform` for the Weblogic Application Server.

    </dd>
    <dt class="hdlist1">`WebSphere`</dt>
    <dd>

    `JtaPlatform` for older versions of the WebSphere Application Server.

    </dd>
    <dt class="hdlist1">`WebSphereExtended`</dt>
    <dd>

    `JtaPlatform` for newer versions of the WebSphere Application Server.

    </dd>
    </dl>
    </div>
    </div>
    <div class="sect2">