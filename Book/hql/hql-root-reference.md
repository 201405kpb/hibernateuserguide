 ### 15.13. Root entity references

    <div class="paragraph">

    A root entity reference, or what JPA calls a `range variable declaration`, is specifically a reference to a mapped entity type from the application.
    It cannot name component/ embeddable types.
    And associations, including collections, are handled in a different manner, as later discussed.

    </div>
    <div class="paragraph">

    The BNF for a root entity reference is:

    </div>
    <div id="hql-root-reference-bnf-example" class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`root_entity_reference ::=
        entity_name [AS] identification_variable`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="hql-root-reference-jpql-fqn-example" class="exampleblock">
    <div class="title">Example 366. Simple query example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from org.hibernate.userguide.model.Person p", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    We see that the query is defining a root entity reference to the `org.hibernate.userguide.model.Person` object model type.
    Additionally, it declares an alias of `p` to that `org.hibernate.userguide.model.Person` reference, which is the identification variable.

    </div>
    <div class="paragraph">

    Usually, the root entity reference represents just the `entity name` rather than the entity class FQN (fully-qualified name).
    By default, the entity name is the unqualified entity class name, here `Person`

    </div>
    <div id="hql-root-reference-jpql-example" class="exampleblock">
    <div class="title">Example 367. Simple query using entity name for root entity reference</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Multiple root entity references can also be specified, even when naming the same entity.

    </div>
    <div id="hql-multiple-root-reference-jpql-example" class="exampleblock">
    <div class="title">Example 368. Simple query using multiple root entity references</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; persons = entityManager.createQuery(
        "select distinct pr, ph " +
        "from Person pr, Phone ph " +
        "where ph.person = pr and ph is not null", Object[].class)
    .getResultList();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select distinct pr1 " +
        "from Person pr1, Person pr2 " +
        "where pr1.id &lt;&gt; pr2.id " +
        "  and pr1.address = pr2.address " +
        "  and pr1.createdOn &lt; pr2.createdOn", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">