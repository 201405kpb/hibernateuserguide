### 23.11. Infinispan properties

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

    `hibernate.cache.infinispan.cfg`
    </td>
    <td class="tableblock halign-left valign-top">

    `org/hibernate/cache/infinispan/
    builder/infinispan-configs.xml`
    </td>
    <td class="tableblock halign-left valign-top">

    Classpath or filesystem resource containing the Infinispan configuration settings.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.cache.infinispan.statistics`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Property name that controls whether Infinispan statistics are enabled. The property value is expected to be a boolean true or false, and it overrides statistic configuration in base Infinispan configuration, if provided.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.cache.infinispan.use_synchronization`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Deprecated setting because Infinispan is designed to always register a `Synchronization` for `TRANSACTIONAL` caches.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.cache.infinispan.cachemanager`
    </td>
    <td class="tableblock halign-left valign-top">

    There is no default value, the user must specify the property.
    </td>
    <td class="tableblock halign-left valign-top">

    Specifies the JNDI name under which the `EmbeddedCacheManager` is bound.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">