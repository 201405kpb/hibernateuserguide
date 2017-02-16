 ### 5.4. Deleting (removing) entities

    <div class="paragraph">

    Entities can also be deleted.

    </div>
    <div id="pc-remove-jpa-example" class="exampleblock">
    <div class="title">Example 230. Deleting an entity with JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`entityManager.remove( person );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-remove-native-example" class="exampleblock">
    <div class="title">Example 231. Deleting an entity with Hibernate API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`session.delete( person );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Hibernate itself can handle deleting detached state.
    JPA, however, disallows it.
    The implication here is that the entity instance passed to the `org.hibernate.Session` delete method can be either in managed or detached state,
    while the entity instance passed to remove on `javax.persistence.EntityManager` must be in managed state.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">