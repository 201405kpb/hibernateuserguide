 #### 25.4.1. Identifiers

    <div class="paragraph">

    When it comes to identifiers, you can either choose a natural id or a synthetic key.

    </div>
    <div class="paragraph">

    For natural identifiers, the **assigned** identifier generator is the right choice.

    </div>
    <div class="paragraph">

    For synthetic keys, the application developer can either choose a randomly generates fixed-size sequence (e.g. UUID) or a natural identifier.
    Natural identifiers are very practical, being more compact than their UUID counterparts, so there are multiple generators to choose from:

    </div>
    <div class="ulist">

*   `IDENTITY`
*   `SEQUENCE`
*   `TABLE`
    </div>
    <div class="paragraph">

    Although the `TABLE` generator addresses the portability concern, in reality, it performs poorly because it requires emulating a database sequence using a separate transaction and row-level locks.
    For this reason, the choice is usually between `IDENTITY` and `SEQUENCE`.

    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    If the underlying database supports sequences, you should always use them for your Hibernate entity identifiers.

    </div>
    <div class="paragraph">

    Only if the relational database does not support sequences (e.g. MySQL 5.7), you should use the `IDENTITY` generators.
    However, you should keep in mind that the `IDENTITY` generators disables JDBC batching for `INSERT` statements.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    If you&#8217;re using the `SEQUENCE` generator, then you should be using the enhanced identifier generators that were enabled by default in Hibernate 5.
    The **pooled** and the **pooled-lo** optimizers are very useful to reduce the number of database roundtrips when writing multiple entities per database transaction.

    </div>
    </div>
    <div class="sect3">
