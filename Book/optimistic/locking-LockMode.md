 ### 10.5. `LockMode` and `LockModeType`

    <div class="paragraph">

    Long before JPA 1.0, Hibernate already defined various explicit locking strategies through its `LockMode` enumeration.
    JPA comes with its own [`LockModeType`](http://docs.oracle.com/javaee/7/api/javax/persistence/LockModeType.html) enumeration which defines similar strategies as the Hibernate-native `LockMode`.

    </div>
    <table class="tableblock frame-all grid-all spread">
    <colgroup>
    <col style="width: %;">
    <col style="width: %;">
    <col style="width: %;">
    </colgroup>
    <thead>
    <tr>
    <th class="tableblock halign-left valign-top">`LockModeType`</th>
    <th class="tableblock halign-left valign-top">`LockMode`</th>
    <th class="tableblock halign-left valign-top">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    `NONE`
    </td>
    <td class="tableblock halign-left valign-top">

    `NONE`
    </td>
    <td class="tableblock halign-left valign-top">

    The absence of a lock. All objects switch to this lock mode at the end of a Transaction. Objects associated with the session via a call to `update()` or `saveOrUpdate()` also start out in this lock mode.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `READ` and `OPTIMISTIC`
    </td>
    <td class="tableblock halign-left valign-top">

    `READ`
    </td>
    <td class="tableblock halign-left valign-top">

    The entity version is checked towards the end of the currently running transaction.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `WRITE` and `OPTIMISTIC_FORCE_INCREMENT`
    </td>
    <td class="tableblock halign-left valign-top">

    `WRITE`
    </td>
    <td class="tableblock halign-left valign-top">

    The entity version is incremented automatically even if the entity has not changed.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `PESSIMISTIC_FORCE_INCREMENT`
    </td>
    <td class="tableblock halign-left valign-top">

    `PESSIMISTIC_FORCE_INCREMENT`
    </td>
    <td class="tableblock halign-left valign-top">

    The entity is locked pessimistically and its version is incremented automatically even if the entity has not changed.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `PESSIMISTIC_READ`
    </td>
    <td class="tableblock halign-left valign-top">

    `PESSIMISTIC_READ`
    </td>
    <td class="tableblock halign-left valign-top">

    The entity is locked pessimistically using a shared lock, if the database supports such a feature. Otherwise, an explicit lock is used.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `PESSIMISTIC_WRITE`
    </td>
    <td class="tableblock halign-left valign-top">

    `PESSIMISTIC_WRITE`, `UPGRADE`
    </td>
    <td class="tableblock halign-left valign-top">

    The entity is locked using an explicit lock.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `PESSIMISTIC_WRITE` with a `javax.persistence.lock.timeout` setting of 0
    </td>
    <td class="tableblock halign-left valign-top">

    `UPGRADE_NOWAIT`
    </td>
    <td class="tableblock halign-left valign-top">

    The lock acquisition request fails fast if the row s already locked.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `PESSIMISTIC_WRITE` with a `javax.persistence.lock.timeout` setting of -2
    </td>
    <td class="tableblock halign-left valign-top">

    `UPGRADE_SKIPLOCKED`
    </td>
    <td class="tableblock halign-left valign-top">

    The lock acquisition request skips the already locked rows. It uses a `SELECT &#8230;&#8203; FOR UPDATE SKIP LOCKED` in Oracle and PostgreSQL 9.5, or `SELECT &#8230;&#8203; with (rowlock, updlock, readpast) in SQL Server`.
    </td>
    </tr>
    </tbody>
    </table>
    <div class="paragraph">

    The explicit user request mentioned above occurs as a consequence of any of the following actions:

    </div>
    <div class="ulist">

*   a call to `Session.load()`, specifying a `LockMode`.
*   a call to `Session.lock()`.
*   a call to `Query.setLockMode()`.
    </div>
    <div class="paragraph">

    If you call `Session.load()` with option `UPGRADE`, `UPGRADE_NOWAIT` or `UPGRADE_SKIPLOCKED`,
    and the requested object is not already loaded by the session, the object is loaded using `SELECT &#8230;&#8203; FOR UPDATE`.

    </div>
    <div class="paragraph">

    If you call `load()` for an object that is already loaded with a less restrictive lock than the one you request, Hibernate calls `lock()` for that object.

    </div>
    <div class="paragraph">

    `Session.lock(`) performs a version number check if the specified lock mode is `READ`, `UPGRADE`, `UPGRADE_NOWAIT` or `UPGRADE_SKIPLOCKED`.
    In the case of `UPGRADE`, `UPGRADE_NOWAIT` or `UPGRADE_SKIPLOCKED`, the `SELECT &#8230;&#8203; FOR UPDATE` syntax is used.

    </div>
    <div class="paragraph">

    If the requested lock mode is not supported by the database, Hibernate uses an appropriate alternate mode instead of throwing an exception.
    This ensures that applications are portable.

    </div>
    </div>
    <div class="sect2">