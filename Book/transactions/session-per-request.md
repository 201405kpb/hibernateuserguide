### 8.7. Session-per-request pattern

    <div class="paragraph">

    This is the most common transaction pattern.
    The term request here relates to the concept of a system that reacts to a series of requests from a client/user.
    Web applications are a prime example of this type of system, though certainly not the only one.
    At the beginning of handling such a request, the application opens a Hibernate Session, starts a transaction, performs all data related work, ends the transaction and closes the Session.
    The crux of the pattern is the one-to-one relationship between the transaction and the Session.

    </div>
    <div class="paragraph">

    Within this pattern there is a common technique of defining a current session to simplify the need of passing this `Session` around to all the application components that may need access to it.
    Hibernate provides support for this technique through the `getCurrentSession` method of the `SessionFactory`.
    The concept of a _current_ session has to have a scope that defines the bounds in which the notion of _current_ is valid.
    This is the purpose of the `org.hibernate.context.spi.CurrentSessionContext` contract.

    </div>
    <div class="paragraph">

    There are 2 reliable defining scopes:

    </div>
    <div class="ulist">

*   First is a JTA transaction because it allows a callback hook to know when it is ending, which gives Hibernate a chance to close the `Session` and clean up.
    This is represented by the `org.hibernate.context.internal.JTASessionContext` implementation of the `org.hibernate.context.spi.CurrentSessionContext` contract.
    Using this implementation, a `Session` will be opened the first time `getCurrentSession` is called within that transaction.
*   Secondly is this application request cycle itself.
    This is best represented with the `org.hibernate.context.internal.ManagedSessionContext` implementation of the `org.hibernate.context.spi.CurrentSessionContext` contract.
    Here an external component is responsible for managing the lifecycle and scoping of a _current_ session.
    At the start of such a scope, `ManagedSessionContext#bind()` method is called passing in the `Session`.
    At the end, its `unbind()` method is called.
    Some common examples of such _external components_ include:
    <div class="ulist">

        *   `javax.servlet.Filter` implementation
    *   AOP interceptor with a pointcut on the service methods
    *   A proxy/interception container
    </div>
    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The `getCurrentSession()` method has one downside in a JTA environment.
    If you use it, `after_statement` connection release mode is also used by default.
    Due to a limitation of the JTA specification, Hibernate cannot automatically clean up any unclosed `ScrollableResults` or `Iterator` instances returned by `scroll()` or `iterate()`.
    Release the underlying database cursor by calling `ScrollableResults#close()` or `Hibernate.close(Iterator)` explicitly from a finally block.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">