 ## 27. Migration

    <div class="sectionbody">
    <div class="paragraph">

    Mapping Configuration methods to the corresponding methods in the new APIs..

    </div>
    <table class="tableblock frame-all grid-all spread">
    <colgroup>
    <col style="width: 50%;">
    <col style="width: 50%;">
    </colgroup>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#addFile`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#addFile`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#add(XmlDocument)`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#add(XmlDocument)`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#addXML`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#addXML`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#addCacheableFile`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#addCacheableFile`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#addURL`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#addURL`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#addInputStream`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#addInputStream`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#addResource`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#addResource`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#addClass`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#addClass`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#addAnnotatedClass`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#addAnnotatedClass`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#addPackage`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#addPackage`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#addJar`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#addJar`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#addDirectory`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#addDirectory`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#registerTypeContributor`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#registerTypeContributor`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#registerTypeOverride`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#registerTypeOverride`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#setProperty`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#setProperty`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#setProperties`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#setProperties`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#addProperties`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#addProperties`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#setNamingStrategy`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#setNamingStrategy`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#setImplicitNamingStrategy`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#setImplicitNamingStrategy`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#setPhysicalNamingStrategy`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#setPhysicalNamingStrategy`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#configure`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#configure`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#setInterceptor`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#setInterceptor`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#setEntityNotFoundDelegate`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#setEntityNotFoundDelegate`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#setSessionFactoryObserver`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#setSessionFactoryObserver`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Configuration#setCurrentTenantIdentifierResolver`
    </td>
    <td class="tableblock halign-left valign-top">

    `Configuration#setCurrentTenantIdentifierResolver`
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    </div>
    <div class="sect1">