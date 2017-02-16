 ## 6. Flushing

    <div class="sectionbody">
    <div class="paragraph">

    Flushing is the process of synchronizing the state of the persistence context with the underlying database.
    The `EntityManager` and the Hibernate `Session` expose a set of methods, through which the application developer can change the persistent state of an entity.

    </div>
    <div class="paragraph">

    The persistence context acts as a transactional write-behind cache, queuing any entity state change.
    Like any write-behind cache, changes are first applied in-memory and synchronized with the database during flush time.
    The flush operation takes every entity state change and translates it to an `INSERT`, `UPDATE` or `DELETE` statement.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Because DML statements are grouped together, Hibernate can apply batching transparently.
    See the [Batching chapter](#batch) for more information.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The flushing strategy is given by the [`flushMode`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/Session.html#getFlushMode--) of the current running Hibernate `Session`.
    Although JPA defines only two flushing strategies ([`AUTO`](https://docs.oracle.com/javaee/7/api/javax/persistence/FlushModeType.html#AUTO) and [`COMMIT`](https://docs.oracle.com/javaee/7/api/javax/persistence/FlushModeType.html#COMMIT)),
    Hibernate has a much broader spectrum of flush types:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">ALWAYS</dt>
    <dd>

    Flushes the `Session` before every query.

    </dd>
    <dt class="hdlist1">AUTO</dt>
    <dd>

    This is the default mode and it flushes the `Session` only if necessary.

    </dd>
    <dt class="hdlist1">COMMIT</dt>
    <dd>

    The `Session` tries to delay the flush until the current `Transaction` is committed, although it might flush prematurely too.

    </dd>
    <dt class="hdlist1">MANUAL</dt>
    <dd>

    The `Session` flushing is delegated to the application, which must call `Session.flush()` explicitly in order to apply the persistence context changes.

    </dd>
    </dl>
    </div>
    <div class="sect2">