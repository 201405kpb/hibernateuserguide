## 13. Caching

    <div class="sectionbody">
    <div class="paragraph">

    At runtime, Hibernate handles moving data into and out of the second-level cache in response to the operations performed by the `Session`, which acts as a transaction-level cache of persistent data.
    Once an entity becomes managed, that object is added to the internal cache of the current persistence context (`EntityManager` or `Session`).
    The persistence context is also called the first-level cache, and it&#8217;s enabled by default.

    </div>
    <div class="paragraph">

    It is possible to configure a JVM-level (`SessionFactory`-level) or even a cluster cache on a class-by-class and collection-by-collection basis.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Be aware that caches are not aware of changes made to the persistent store by other applications.
    They can, however, be configured to regularly expire cached data.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="sect2">