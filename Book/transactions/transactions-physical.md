 ### 8.1. Physical Transactions

    <div class="paragraph">

    Hibernate uses the JDBC API for persistence. In the world of Java there are two well-defined mechanism for dealing with transactions in JDBC: JDBC itself and JTA.
    Hibernate supports both mechanisms for integrating with transactions and allowing applications to manage physical transactions.

    </div>
    <div class="paragraph">

    Transaction handling per `Session` is handled by the `org.hibernate.resource.transaction.spi.TransactionCoordinator` contract,
    which are built by the `org.hibernate.resource.transaction.spi.TransactionCoordinatorBuilder` service.
    `TransactionCoordinatorBuilder` represents a strategy for dealing with transactions whereas TransactionCoordinator represents one instance of that strategy related to a Session.
    Which `TransactionCoordinatorBuilder` implementation to use is defined by the `hibernate.transaction.coordinator_class` setting.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`jdbc` (the default for non-JPA applications)</dt>
    <dd>

    Manages transactions via calls to `java.sql.Connection`

    </dd>
    <dt class="hdlist1">`jta`</dt>
    <dd>

    Manages transactions via JTA. See [Java EE bootstrapping](#bootstrap-jpa-compliant)

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    If a JPA application does not provide a setting for `hibernate.transaction.coordinator_class`, Hibernate will
    automatically build the proper transaction coordinator based on the transaction type for the persistence unit.

    </div>
    <div class="paragraph">

    If a non-JPA application does not provide a setting for `hibernate.transaction.coordinator_class`, Hibernate
    will use `jdbc` as the default. This default will cause problems if the application actually uses JTA-based transactions.
    A non-JPA application that uses JTA-based transactions should explicitly set `hibernate.transaction.coordinator_class=jta`
    or provide a custom `org.hibernate.resource.transaction.TransactionCoordinatorBuilder` that builds a
    `org.hibernate.resource.transaction.TransactionCoordinator` that properly coordinates with JTA-based transactions.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    For details on implementing a custom `TransactionCoordinatorBuilder`, or simply better understanding how it works, see the Integrations Guide.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Hibernate uses JDBC connections and JTA resources directly, without adding any additional locking behavior.
    Hibernate does not lock objects in memory.
    The behavior defined by the isolation level of your database transactions does not change when you use Hibernate.
    The Hibernate `Session` acts as a transaction-scoped cache providing repeatable reads for lookup by identifier and queries that result in loading entities.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    To reduce lock contention in the database, the physical database transaction needs to be as short as possible.
    Long database transactions prevent your application from scaling to a highly-concurrent load.
    Do not hold a database transaction open during end-user-level work, but open it after the end-user-level work is finished.
    This is concept is referred to as `transactional write-behind`.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">