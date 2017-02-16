 ### 8.8. Conversations

    <div class="paragraph">

    The session-per-request pattern is not the only valid way of designing units of work.
    Many business processes require a whole series of interactions with the user that are interleaved with database accesses.
    In web and enterprise applications, it is not acceptable for a database transaction to span a user interaction. Consider the following example:

    </div>
    <div class="paragraph">

    The first screen of a dialog opens.
    The data seen by the user is loaded in a particular `Session` and database transaction.
    The user is free to modify the objects.

    </div>
    <div class="paragraph">

    The user uses a UI element to save their work after five minutes of editing.
    The modifications are made persistent.
    The user also expects to have exclusive access to the data during the edit session.

    </div>
    <div class="paragraph">

    Even though we have multiple databases access here, from the point of view of the user, this series of steps represents a single unit of work.
    There are many ways to implement this in your application.

    </div>
    <div class="paragraph">

    A first naive implementation might keep the `Session` and database transaction open while the user is editing, using database-level locks to prevent other users from modifying the same data and to guarantee isolation and atomicity.
    This is an anti-pattern because lock contention is a bottleneck which will prevent scalability in the future.

    </div>
    <div class="paragraph">

    Several database transactions are used to implement the conversation.
    In this case, maintaining isolation of business processes becomes the partial responsibility of the application tier.
    A single conversation usually spans several database transactions.
    These multiple database accesses can only be atomic as a whole if only one of these database transactions (typically the last one) stores the updated data.
    All others only read data.
    A common way to receive this data is through a wizard-style dialog spanning several request/response cycles.
    Hibernate includes some features which make this easy to implement.

    </div>
    <table class="tableblock frame-all grid-all spread">
    <colgroup>
    <col style="width: %;">
    <col style="width: %;">
    </colgroup>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    Automatic Versioning
    </td>
    <td class="tableblock halign-left valign-top">

    Hibernate can perform automatic optimistic concurrency control for you.
    It can automatically detect (at the end of the conversation) if a concurrent modification occurred during user think time.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Detached Objects
    </td>
    <td class="tableblock halign-left valign-top">

    If you decide to use the session-per-request pattern, all loaded instances will be in the detached state during user think time.
    Hibernate allows you to reattach the objects and persist the modifications.
    The pattern is called session-per-request-with-detached-objects.
    Automatic versioning is used to isolate concurrent modifications.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Extended `Session`
    </td>
    <td class="tableblock halign-left valign-top">

    The Hibernate `Session` can be disconnected from the underlying JDBC connection after the database transaction has been committed and reconnected when a new client request occurs.
    This pattern is known as session-per-conversation and makes even reattachment unnecessary.
    Automatic versioning is used to isolate concurrent modifications and the `Session` will not be allowed to flush automatically, only explicitly.
    </td>
    </tr>
    </tbody>
    </table>
    <div class="paragraph">

    Session-per-request-with-detached-objects and session-per-conversation each have advantages and disadvantages.

    </div>
    </div>
    <div class="sect2">