### 15.6. Statement types

    <div class="paragraph">

    Both HQL and JPQL allow `SELECT`, `UPDATE` and `DELETE` statements to be performed.
    HQL additionally allows `INSERT` statements, in a form similar to a SQL `INSERT FROM SELECT`.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Care should be taken as to when a `UPDATE` or `DELETE` statement is executed.

    </div>
    <div class="quoteblock">
    > <div class="paragraph">
> 
>     Caution should be used when executing bulk update or delete operations because they may result in
>     inconsistencies between the database and the entities in the active persistence context. In general, bulk
>     update and delete operations should only be performed within a transaction in a new persistence con
>     text or before fetching or accessing entities whose state might be affected by such operations.
> 
>     </div>
    <div class="attribution">
    &#8212; Section 4.10 of the JPA 2.0 Specification
    </div>
    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">