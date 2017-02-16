 ### 21.4. Choosing an audit strategy

    <div class="paragraph">

    After the basic configuration, it is important to choose the audit strategy that will be used to persist and retrieve audit information.
    There is a trade-off between the performance of persisting and the performance of querying the audit information.
    Currently, there are two audit strategies.

    </div>
    <div class="olist arabic">

1.  The default audit strategy persists the audit data together with a start revision.
    For each row inserted, updated or deleted in an audited table, one or more rows are inserted in the audit tables, together with the start revision of its validity.
    Rows in the audit tables are never updated after insertion.
    Queries of audit information use subqueries to select the applicable rows in the audit tables.
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">
    </td>
    <td class="content">
    These subqueries are notoriously slow and difficult to index.
    </td>
    </tr>
    </table>
    </div>
2.  The alternative is a validity audit strategy.
    This strategy stores the start-revision and the end-revision of audit information.
    For each row inserted, updated or deleted in an audited table, one or more rows are inserted in the audit tables, together with the start revision of its validity.
    But at the same time the end-revision field of the previous audit rows (if available) are set to this revision.
    Queries on the audit information can then use 'between start and end revision' instead of subqueries as used by the default audit strategy.
    <div class="literalblock">
    <div class="content">
    <pre>The consequence of this strategy is that persisting audit information will be a bit slower because of the extra updates involved,
    but retrieving audit information will be a lot faster.
    This can be improved even further by adding extra indexes.</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">
