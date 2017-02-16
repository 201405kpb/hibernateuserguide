 ## 5. Persistence Contexts

    <div class="sectionbody">
    <div class="paragraph">

    Both the `org.hibernate.Session` API and `javax.persistence.EntityManager` API represent a context for dealing with persistent data.
    This concept is called a `persistence context`.
    Persistent data has a state in relation to both a persistence context and the underlying database.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`transient`</dt>
    <dd>

    the entity has just been instantiated and is not associated with a persistence context.
    It has no persistent representation in the database and typically no identifier value has been assigned (unless the _assigned_ generator was used).

    </dd>
    <dt class="hdlist1">`managed`, or `persistent`</dt>
    <dd>

    the entity has an associated identifier and is associated with a persistence context.
    It may or may not physically exist in the database yet.

    </dd>
    <dt class="hdlist1">`detached`</dt>
    <dd>

    the entity has an associated identifier, but is no longer associated with a persistence context (usually because the persistence context was closed or the instance was evicted from the context)

    </dd>
    <dt class="hdlist1">`removed`</dt>
    <dd>

    the entity has an associated identifier and is associated with a persistence context, however it is scheduled for removal from the database.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    Much of the `org.hibernate.Session` and `javax.persistence.EntityManager` methods deal with moving entities between these states.

    </div>
    <div class="sect2">