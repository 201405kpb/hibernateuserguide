### 8.9. Session-per-application

    <div class="paragraph">

    The _session-per-application_ is also considered an anti-pattern.
    The Hibernate `Session`, like the JPA `EntityManager`, is not a thread-safe object and it is intended to be confined to a single thread at once.
    If the `Session` is shared among multiple threads, there will be race conditions as well as visibility issues , so beware of this.

    </div>
    <div class="paragraph">

    An exception thrown by Hibernate means you have to rollback your database transaction and close the `Session` immediately.
    If your `Session` is bound to the application, you have to stop the application.
    Rolling back the database transaction does not put your business objects back into the state they were at the start of the transaction.
    This means that the database state and the business objects will be out of sync.
    Usually, this is not a problem because exceptions are not recoverable and you will have to start over after rollback anyway.

    </div>
    <div class="paragraph">

    The `Session` caches every object that is in a persistent state (watched and checked for dirty state by Hibernate).
    If you keep it open for a long time or simply load too much data, it will grow endlessly until you get an `OutOfMemoryException`.
    One solution is to call `clear()` and `evict()` to manage the `Session` cache, but you should consider a Stored Procedure if you need mass data operations.
    Some solutions are shown in the [Batching chapter](#batch).
    Keeping a `Session` open for the duration of a user session also means a higher probability of stale data.

    </div>
    </div>
    </div>
    </div>
    <div class="sect1">
