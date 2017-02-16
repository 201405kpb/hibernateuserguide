 ### 21.15. Understanding the Envers Schema

    <div class="paragraph">

    For each audited entity (that is, for each entity containing at least one audited field), an audit table is created.
    By default, the audit table&#8217;s name is created by adding an "_AUD" suffix to the original table name,
    but this can be overridden by specifying a different suffix/prefix in the configuration properties or per-entity using the `@org.hibernate.envers.AuditTable` annotation.

    </div>
    <div class="paragraph">

    The audit table contains the following columns:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">id</dt>
    <dd>

    `id` of the original entity (this can be more then one column in the case of composite primary keys)

    </dd>
    <dt class="hdlist1">revision number</dt>
    <dd>

    an integer, which matches to the revision number in the revision entity table.

    </dd>
    <dt class="hdlist1">revision type</dt>
    <dd>

    a small integer

    </dd>
    <dt class="hdlist1">audited fields</dt>
    <dd>

    propertied from the original entity being audited

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    The primary key of the audit table is the combination of the original id of the entity and the revision number, so there can be at most one historic entry for a given entity instance at a given revision.

    </div>
    <div class="paragraph">

    The current entity data is stored in the original table and in the audit table.
    This is a duplication of data, however as this solution makes the query system much more powerful, and as memory is cheap, hopefully this won&#8217;t be a major drawback for the users.
    A row in the audit table with entity id `ID`, revision `N` and data `D` means: entity with id `ID` has data `D` from revision `N` upwards.
    Hence, if we want to find an entity at revision `M`, we have to search for a row in the audit table, which has the revision number smaller or equal to `M`, but as large as possible.
    If no such row is found, or a row with a "deleted" marker is found, it means that the entity didn&#8217;t exist at that revision.

    </div>
    <div class="paragraph">

    The "revision type" field can currently have three values: `0`, `1` and `2`, which means `ADD`, `MOD` and `DEL`, respectively.
    A row with a revision of type `DEL` will only contain the id of the entity and no data (all fields `NULL`), as it only serves as a marker saying "this entity was deleted at that revision".

    </div>
    <div class="paragraph">

    Additionally, there is a revision entity table which contains the information about the global revision.
    By default the generated table is named `REVINFO` and contains just two columns: `ID` and `TIMESTAMP`.
    A row is inserted into this table on each new revision, that is, on each commit of a transaction, which changes audited data.
    The name of this table can be configured, the name of its columns as well as adding additional columns can be achieved as discussed in [Revision Log](#envers-revisionlog).

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    While global revisions are a good way to provide correct auditing of relations, some people have pointed out that this may be a bottleneck in systems, where data is very often modified.
    One viable solution is to introduce an option to have an entity "locally revisioned", that is revisions would be created for it independently.
    This woulld not enable correct versioning of relations, but it would work without the `REVINFO` table.
    Another possibility is to introduce a notion of "revisioning groups", which would group entities sharing the same revision numbering.
    Each such group would have to consist of one or more strongly connected components belonging to the entity graph induced by relations between entities.
    Your opinions on the subject are very welcome on the forum! :)

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">