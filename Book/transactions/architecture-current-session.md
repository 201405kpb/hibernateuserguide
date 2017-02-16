### 8.4. Contextual sessions

    <div class="paragraph">

    Most applications using Hibernate need some form of _contextual_ session, where a given session is in effect throughout the scope of a given context.
    However, across applications the definition of what constitutes a context is typically different; different contexts define different scopes to the notion of current.
    Applications using Hibernate prior to version 3.0 tended to utilize either home-grown `ThreadLocal`-based contextual sessions, helper classes such as `HibernateUtil`, or utilized third-party frameworks, such as Spring or Pico, which provided proxy/interception-based contextual sessions.

    </div>
    <div class="paragraph">

    Starting with version 3.0.1, Hibernate added the `SessionFactory.getCurrentSession()` method.
    Initially, this assumed usage of `JTA` transactions, where the `JTA` transaction defined both the scope and context of a current session.
    Given the maturity of the numerous stand-alone `JTA TransactionManager` implementations, most, if not all, applications should be using `JTA` transaction management, whether or not they are deployed into a `J2EE` container.
    Based on that, the `JTA`-based contextual sessions are all you need to use.

    </div>
    <div class="paragraph">

    However, as of version 3.1, the processing behind `SessionFactory.getCurrentSession()` is now pluggable.
    To that end, a new extension interface, `org.hibernate.context.spi.CurrentSessionContext`,
    and a new configuration parameter, `hibernate.current_session_context_class`, have been added to allow pluggability of the scope and context of defining current sessions.

    </div>
    <div class="paragraph">

    See the [Javadocs](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/context/spi/CurrentSessionContext.html) for the `org.hibernate.context.spi.CurrentSessionContext` interface for a detailed discussion of its contract.
    It defines a single method, `currentSession()`, by which the implementation is responsible for tracking the current contextual session.
    Out-of-the-box, Hibernate comes with three implementations of this interface:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`org.hibernate.context.internal.JTASessionContext`</dt>
    <dd>

    current sessions are tracked and scoped by a `JTA` transaction.
    The processing here is exactly the same as in the older JTA-only approach. See the [Javadocs](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/context/internal/JTASessionContext.html) for more details.

    <div class="ulist">

*   `org.hibernate.context.internal.ThreadLocalSessionContext`:current sessions are tracked by thread of execution. See the [Javadocs](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/context/internal/ThreadLocalSessionContext.html) for more details.
*   `org.hibernate.context.internal.ManagedSessionContext`: current sessions are tracked by thread of execution.
    However, you are responsible to bind and unbind a `Session` instance with static methods on this class: it does not open, flush, or close a `Session`. See the [Javadocs](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/context/internal/ManagedSessionContext.html) for details.
    </div>
    </dd>
    </dl>
    </div>
    <div class="paragraph">

    Typically, the value of this parameter would just name the implementation class to use.
    For the three out-of-the-box implementations, however, there are three corresponding short names: _jta_, _thread_, and _managed_.

    </div>
    <div class="paragraph">

    The first two implementations provide a _one session - one database transaction_ programming model.
    This is also known and used as _session-per-request_.
    The beginning and end of a Hibernate session is defined by the duration of a database transaction.
    If you use programmatic transaction demarcation in plain Java SE without JTA, you are advised to use the Hibernate `Transaction` API to hide the underlying transaction system from your code.
    If you use JTA, you can utilize the JTA interfaces to demarcate transactions.
    If you execute in an EJB container that supports CMT, transaction boundaries are defined declaratively and you do not need any transaction or session demarcation operations in your code. Refer to [Transactions and concurrency control](#transactions) for more information and code examples.

    </div>
    <div class="paragraph">

    The `hibernate.current_session_context_class` configuration parameter defines which `org.hibernate.context.spi.CurrentSessionContext` implementation should be used.
    For backwards compatibility, if this configuration parameter is not set but a `org.hibernate.engine.transaction.jta.platform.spi.JtaPlatform` is configured, Hibernate will use the `org.hibernate.context.internal.JTASessionContext`.

    </div>
    </div>
    <div class="sect2">