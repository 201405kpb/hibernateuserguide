### 17.6. Alias and property references

    <div class="paragraph">

    In most cases the above alias injection is needed.
    For queries relating to more complex mappings, like composite properties, inheritance discriminators, collections etc., you can use specific aliases that allow Hibernate to inject the proper aliases.

    </div>
    <div class="paragraph">

    The following table shows the different ways you can use the alias injection.
    Please note that the alias names in the result are simply examples, each alias will have a unique and probably different name when used.

    </div>
    <table class="tableblock frame-all grid-all spread">
    <caption class="title">Table 8. Alias injection names</caption>
    <colgroup>
    <col style="width: 23%;">
    <col style="width: 22%;">
    <col style="width: 55%;">
    </colgroup>
    <thead>
    <tr>
    <th class="tableblock halign-left valign-top">Description</th>
    <th class="tableblock halign-left valign-top">Syntax</th>
    <th class="tableblock halign-left valign-top">Example</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    A simple property
    </td>
    <td class="tableblock halign-left valign-top">

    `{[aliasname].[propertyname]`
    </td>
    <td class="tableblock halign-left valign-top">

    `A_NAME as {item.name}`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    A composite property
    </td>
    <td class="tableblock halign-left valign-top">

    `{[aliasname].[componentname].[propertyname]}`
    </td>
    <td class="tableblock halign-left valign-top">

    `CURRENCY as {item.amount.currency}, VALUE as {item.amount.value}`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Discriminator of an entity
    </td>
    <td class="tableblock halign-left valign-top">

    `{[aliasname].class}`
    </td>
    <td class="tableblock halign-left valign-top">

    `DISC as {item.class}`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    All properties of an entity
    </td>
    <td class="tableblock halign-left valign-top">

    `{[aliasname].*}`
    </td>
    <td class="tableblock halign-left valign-top">

    `{item.*}`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    A collection key
    </td>
    <td class="tableblock halign-left valign-top">

    `{[aliasname].key}`
    </td>
    <td class="tableblock halign-left valign-top">

    `ORGID as {coll.key}`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    The id of an collection
    </td>
    <td class="tableblock halign-left valign-top">

    `{[aliasname].id}`
    </td>
    <td class="tableblock halign-left valign-top">

    `EMPID as {coll.id}`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    The element of an collection
    </td>
    <td class="tableblock halign-left valign-top">

    `{[aliasname].element}`
    </td>
    <td class="tableblock halign-left valign-top">

    `XID as {coll.element}`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    property of the element in the collection
    </td>
    <td class="tableblock halign-left valign-top">

    `{[aliasname].element.[propertyname]}`
    </td>
    <td class="tableblock halign-left valign-top">

    `NAME as {coll.element.name}`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    All properties of the element in the collection
    </td>
    <td class="tableblock halign-left valign-top">

    `{[aliasname].element.*}`
    </td>
    <td class="tableblock halign-left valign-top">

    `{coll.element.*}`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    All properties of the collection
    </td>
    <td class="tableblock halign-left valign-top">

    `{[aliasname].*}`
    </td>
    <td class="tableblock halign-left valign-top">

    `{coll.*}`
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">