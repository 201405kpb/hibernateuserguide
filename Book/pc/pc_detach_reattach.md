 #### 5.10.1. Reattaching detached data

    <div class="paragraph">

    Reattachment is the process of taking an incoming entity instance that is in detached state and re-associating it with the current persistence context.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    JPA does not provide for this model. This is only available through Hibernate `org.hibernate.Session`.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div id="pc-detach-reattach-lock-example" class="exampleblock">
    <div class="title">Example 247. Reattaching a detached entity using `lock`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = session.byId( Person.class ).load( personId );
    //Clear the Session so the person entity becomes detached
    session.clear();
    person.setName( "Mr. John Doe" );

    session.lock( person, LockMode.NONE );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-detach-reattach-saveOrUpdate-example" class="exampleblock">
    <div class="title">Example 248. Reattaching a detached entity using `saveOrUpdate`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = session.byId( Person.class ).load( personId );
    //Clear the Session so the person entity becomes detached
    session.clear();
    person.setName( "Mr. John Doe" );

    session.saveOrUpdate( person );`</pre>
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

    The method name `update` is a bit misleading here.
    It does not mean that an `SQL` `UPDATE` is immediately performed.
    It does, however, mean that an `SQL` `UPDATE` will be performed when the persistence context is flushed since Hibernate does not know its previous state against which to compare for changes.
    If the entity is mapped with `select-before-update`, Hibernate will pull the current state from the database and see if an update is needed.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Provided the entity is detached, `update` and `saveOrUpdate` operate exactly the same.

    </div>
    </div>
    <div class="sect3">
