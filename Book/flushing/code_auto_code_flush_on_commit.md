 #### 6.1.1. `AUTO` flush on commit

    <div class="paragraph">

    In the following example, an entity is persisted and then the transaction is committed.

    </div>
    <div id="flushing-auto-flush-commit-example" class="exampleblock">
    <div class="title">Example 261. Automatic flushing on commit</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`entityManager = entityManagerFactory().createEntityManager();
    txn = entityManager.getTransaction();
    txn.begin();

    Person person = new Person( "John Doe" );
    entityManager.persist( person );
    log.info( "Entity is in persisted state" );

    txn.commit();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`--INFO: Entity is in persisted state
    INSERT INTO Person (name, id) VALUES ('John Doe', 1)`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Hibernate logs the message prior to inserting the entity because the flush only occurred during transaction commit.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    This is valid for the `SEQUENCE` and `TABLE` identifier generators.
    The `IDENTITY` generator must execute the insert right after calling `persist()`.
    For details, see the discussion of generators in [_Identifier generators_](#identifiers).

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">