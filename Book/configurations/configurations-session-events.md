### 23.16. Session events

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

    `hibernate.session.events.auto`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Fully qualified class name implementing the `SessionEventListener` interface.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.session_factory.interceptor` or `hibernate.ejb.interceptor`
    </td>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.EmptyInterceptor` (default value)
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Names a [https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/Interceptor`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/Interceptor`) implementation to be applied to every `Session` created by the current `org.hibernate.SessionFactory`

    </div>
    <div class="paragraph">

    Can reference:

    </div>
    <div class="ulist">

*   `Interceptor` instance
*   `Interceptor` implementation `Class` object reference
*   `Interceptor` implementation class name
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.ejb.interceptor.session_scoped`
    </td>
    <td class="tableblock halign-left valign-top">

    fully-qualified class name or class reference
    </td>
    <td class="tableblock halign-left valign-top">

    An optional Hibernate interceptor.

    The interceptor instance is specific to a given Session instance (and hence is not thread-safe) has to implement `org.hibernate.Interceptor` and have a no-arg constructor.

    This property can not be combined with `hibernate.ejb.interceptor`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.ejb.session_factory_observer`
    </td>
    <td class="tableblock halign-left valign-top">

    fully-qualified class name or class reference
    </td>
    <td class="tableblock halign-left valign-top">

    Specifies a `SessionFactoryObserver` to be applied to the SessionFactory. The class must have a no-arg constructor.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.ejb.event`
    </td>
    <td class="tableblock halign-left valign-top">

    `hibernate.ejb.event.pre-load` = `com.acme.SecurityListener,com.acme.AuditListener`
    </td>
    <td class="tableblock halign-left valign-top">

    Event listener list for a given event type. The list of event listeners is a comma separated fully qualified class name list.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">