 ### 21.14. Conditional auditing

    <div class="paragraph">

    Envers persists audit data in reaction to various Hibernate events (e.g. `post update`, `post insert`, and so on), using a series of event listeners from the `org.hibernate.envers.event.spi` package.
    By default, if the Envers jar is in the classpath, the event listeners are auto-registered with Hibernate.

    </div>
    <div class="paragraph">

    Conditional auditing can be implemented by overriding some of the Envers event listeners.
    To use customized Envers event listeners, the following steps are needed:

    </div>
    <div class="olist arabic">

1.  Turn off automatic Envers event listeners registration by setting the `hibernate.listeners.envers.autoRegister` Hibernate property to `false`.
2.  Create subclasses for appropriate event listeners.
    For example, if you want to conditionally audit entity insertions, extend the `org.hibernate.envers.event.spi.EnversPostInsertEventListenerImpl` class.
    Place the conditional-auditing logic in the subclasses, call the super method if auditing should be performed.
3.  Create your own implementation of `org.hibernate.integrator.spi.Integrator`, similar to `org.hibernate.envers.boot.internal.EnversIntegrator`.
    Use your event listener classes instead of the default ones.
4.  For the integrator to be automatically used when Hibernate starts up, you will need to add a `META-INF/services/org.hibernate.integrator.spi.Integrator` file to your jar.
    The file should contain the fully qualified name of the class implementing the interface.
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The use of `hibernate.listeners.envers.autoRegister` has been deprecated.  A new configuration setting
    `hibernate.envers.autoRegisterListeners` should be used instead.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">