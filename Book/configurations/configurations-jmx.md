### 23.17. JMX settings

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

    `hibernate.jmx.enabled`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Enable JMX.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jmx.usePlatformServer`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Uses the platform MBeanServer as returned by `ManagementFactory#getPlatformMBeanServer()`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jmx.agentId`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    The agent identifier of the associated `MBeanServer`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jmx.defaultDomain`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    The domain name of the associated `MBeanServer`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jmx.sessionFactoryName`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    The `SessionFactory` name appended to the object name the Manageable Bean is registered with. If null, the `hibernate.session_factory_name` configuration value is used.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.core
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    The default object domain appended to the object name the Manageable Bean is registered with.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">