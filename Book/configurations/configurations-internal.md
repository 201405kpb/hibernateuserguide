 ### 23.24. Internal properties

    <div class="paragraph">

    The following configuration properties are used internally, and you shouldn&#8217;t probably have to configured them in your application.

    </div>
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

    `hibernate.enable_specj_proprietary_syntax`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Enable or disable the SpecJ proprietary mapping syntax which differs from JPA specification. Used during performance testing only.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.temp.use_jdbc_metadata_defaults`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` (default value) or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    This setting is used to control whether we should consult the JDBC metadata to determine certain Settings default values when the database may not be available (mainly in tools usage).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection_provider.injection_data`
    </td>
    <td class="tableblock halign-left valign-top">

    `java.util.Map`
    </td>
    <td class="tableblock halign-left valign-top">

    Connection provider settings to be injected in the currently configured connection provider.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jandex_index`
    </td>
    <td class="tableblock halign-left valign-top">

    `org.jboss.jandex.Index`
    </td>
    <td class="tableblock halign-left valign-top">

    Names a Jandex `org.jboss.jandex.Index` instance to use.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    </div>
    </div>
    <div class="sect1">