 ### 6.5. Flush operation order

    <div class="paragraph">

    From a database perspective, a row state can be altered using either an `INSERT`, an `UPDATE` or a `DELETE` statement.
    Because entity state changes are automatically converted to SQL statements, it&#8217;s important to know which entity actions are associated to a given SQL statement.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`INSERT`</dt>
    <dd>

    The `INSERT` statement is generated either by the `EntityInsertAction` or `EntityIdentityInsertAction`. These actions are scheduled by the `persist` operation, either explicitly or through cascading the `PersistEvent` from a parent to a child entity.

    </dd>
    <dt class="hdlist1">`DELETE`</dt>
    <dd>

    The `DELETE` statement is generated by the `EntityDeleteAction` or `OrphanRemovalAction`.

    </dd>
    <dt class="hdlist1">`UPDATE`</dt>
    <dd>

    The `UPDATE` statement is generated by `EntityUpdateAction` during flushing if the managed entity has been marked modified. The dirty checking mechanism is responsible for determining if a managed entity has been modified since it was first loaded.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    Hibernate does not execute the SQL statements in the order of their associated entity state operations.

    </div>
    <div class="paragraph">

    To visualize how this works, consider the following example:

    </div>
    <div id="flushing-order-example" class="exampleblock">
    <div class="title">Example 272. Flush operation order</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = entityManager.find( Person.class, 1L);
    entityManager.remove(person);

    Person newPerson = new Person( );
    newPerson.setId( 2L );
    newPerson.setName( "John Doe" );
    entityManager.persist( newPerson );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Person (name, id)
    VALUES ('John Doe', 2L)

    DELETE FROM Person WHERE id = 1`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Even if we removed the first entity and then persist a new one, Hibernate is going to execute the `DELETE` statement after the `INSERT`.

    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The order in which SQL statements are executed is given by the `ActionQueue` and not by the order in which entity state operations have been previously defined.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The `ActionQueue` executes all operations in the following order:

    </div>
    <div class="olist arabic">

1.  `OrphanRemovalAction`
2.  `EntityInsertAction` or `EntityIdentityInsertAction`
3.  `EntityUpdateAction`
4.  `CollectionRemoveAction`
5.  `CollectionUpdateAction`
6.  `CollectionRecreateAction`
7.  `EntityDeleteAction`
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">