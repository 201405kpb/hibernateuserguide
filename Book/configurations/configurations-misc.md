### 23.19. ClassLoaders properties

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

    `hibernate.classLoaders`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Used to define a `java.util.Collection&lt;ClassLoader&gt;` or the `ClassLoader` instance Hibernate should use for class-loading and resource-lookups.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.classLoader.application`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Names the `ClassLoader` used to load user application classes.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.classLoader.resources`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Names the `ClassLoader` Hibernate should use to perform resource loading.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.classLoader.hibernate`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Names the `ClassLoader` responsible for loading Hibernate classes.  By default this is the `ClassLoader` that loaded this class.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.classLoader.environment`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Names the `ClassLoader` used when Hibernate is unable to locates classes on the `hibernate.classLoader.application` or `hibernate.classLoader.hibernate`.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">