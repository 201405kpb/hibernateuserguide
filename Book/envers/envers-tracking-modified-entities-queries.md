 ### 21.12. Querying for entities modified in a given revision

    <div class="paragraph">

    The basic query allows retrieving entity names and corresponding Java classes changed in a specified revision:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`modifiedEntityTypes = getAuditReader()
    	.getCrossTypeRevisionChangesReader()
    	.findEntityTypes( revisionNumber );`</pre>
    </div>
    </div>
    <div class="paragraph">

    Other queries (also accessible from `org.hibernate.envers.CrossTypeRevisionChangesReader`):

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`List&lt;Object&gt; findEntities( Number )`</dt>
    <dd>

    Returns snapshots of all audited entities changed (added, updated and removed) in a given revision.
    Executes `N+1` SQL queries, where `N` is a number of different entity classes modified within specified revision.

    </dd>
    <dt class="hdlist1">`List&lt;Object&gt; findEntities( Number, RevisionType )`</dt>
    <dd>

    Returns snapshots of all audited entities changed (added, updated or removed) in a given revision filtered by modification type.
    Executes `N+1` SQL queries, where `N` is a number of different entity classes modified within specified revision.

    </dd>
    <dt class="hdlist1">`Map&lt;RevisionType, List&lt;Object&gt;&gt; findEntitiesGroupByRevisionType( Number )`</dt>
    <dd>

    Returns a map containing lists of entity snapshots grouped by modification operation (e.g. addition, update and removal).
    Executes `3N+1` SQL queries, where `N` is a number of different entity classes modified within specified revision.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    Note that methods described above can be legally used only when the default mechanism of tracking changed entity names is enabled (see [Tracking entity names modified during revisions](#envers-tracking-modified-entities-revchanges)).

    </div>
    </div>
    <div class="sect2">