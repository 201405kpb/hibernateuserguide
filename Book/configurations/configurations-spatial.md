 ### 23.23. Spatial properties

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

    `hibernate.integration.spatial.enabled`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` (default value) or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Enable or disable the Hibernate Spatial `Service` integration.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.spatial.connection_finder`
    </td>
    <td class="tableblock halign-left valign-top">

    `org.geolatte.geom.codec.db.oracle.DefaultConnectionFinder`
    </td>
    <td class="tableblock halign-left valign-top">

    Define the fully-qualified name of class implementing the `org.geolatte.geom.codec.db.oracle.ConnectionFinder` interface.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">