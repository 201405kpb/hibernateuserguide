 ### 21.8. Queries

    <div class="paragraph">

    You can think of historic data as having two dimensions:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">horizontal</dt>
    <dd>

    is the state of the database at a given revision. Thus, you can query for entities as they were at revision N.

    </dd>
    <dt class="hdlist1">vertical</dt>
    <dd>

    are the revisions, at which entities changed. Hence, you can query for revisions, in which a given entity changed.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    The queries in Envers are similar to Hibernate Criteria queries, so if you are common with them, using Envers queries will be much easier.

    </div>
    <div class="paragraph">

    The main limitation of the current queries implementation is that you cannot traverse relations.
    You can only specify constraints on the ids of the related entities, and only on the "owning" side of the relation.
    This however will be changed in future releases.

    </div>
    <div class="paragraph">

    Please note, that queries on the audited data will be in many cases much slower than corresponding queries on "live" data, as they involve correlated subselects.

    </div>
    <div class="paragraph">

    Queries are improved both in terms of speed and possibilities, when using the valid-time audit strategy, that is when storing both start and end revisions for entities. See [Configuration](#envers-configuration).

    </div>
    </div>
    <div class="sect2">