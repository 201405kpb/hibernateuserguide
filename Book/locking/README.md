## 10. Locking

    <div class="sectionbody">
    <div class="paragraph">

    In a relational database, locking refers to actions taken to prevent data from changing between the time it is read and the time is used.

    </div>
    <div class="paragraph">

    Your locking strategy can be either optimistic or pessimistic.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">Optimistic</dt>
    <dd>

    [Optimistic locking](http://en.wikipedia.org/wiki/Optimistic_locking) assumes that multiple transactions can complete without affecting each other,
    and that therefore transactions can proceed without locking the data resources that they affect.
    Before committing, each transaction verifies that no other transaction has modified its data.
    If the check reveals conflicting modifications, the committing transaction rolls back.

    </dd>
    <dt class="hdlist1">Pessimistic</dt>
    <dd>

    Pessimistic locking assumes that concurrent transactions will conflict with each other,
    and requires resources to be locked after they are read and only unlocked after the application has finished using the data.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    Hibernate provides mechanisms for implementing both types of locking in your applications.

    </div>
    <div class="sect2">