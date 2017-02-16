  ### 8.6. Session-per-operation anti-pattern

    <div class="paragraph">

    This is an anti-pattern of opening and closing a `Session` for each database call in a single thread.
    It is also an anti-pattern in terms of database transactions.
    Group your database calls into a planned sequence.
    In the same way, do not auto-commit after every SQL statement in your application.
    Hibernate disables, or expects the application server to disable, auto-commit mode immediately.
    Database transactions are never optional.
    All communication with a database must be encapsulated by a transaction.
    Avoid auto-commit behavior for reading data because many small transactions are unlikely to perform better than one clearly-defined unit of work, and are more difficult to maintain and extend.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Using auto-commit does not circumvent database transactions.
    Instead, when in auto-commit mode, JDBC drivers simply perform each call in an implicit transaction call.
    It is as if your application called commit after each and every JDBC call.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">
