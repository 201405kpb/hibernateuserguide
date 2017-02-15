#### 2.8.1. Collections as a value type

    <div class="paragraph">

    Value and embeddable type collections have a similar behavior as simple value types because they are automatically persisted when referenced by a persistent object and automatically deleted when unreferenced.
    If a collection is passed from one persistent object to another, its elements might be moved from one table to another.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Two entities cannot share a reference to the same collection instance.
    Collection-valued properties do not support null value semantics because Hibernate does not distinguish between a null collection reference and an empty collection.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">