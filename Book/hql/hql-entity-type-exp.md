    ### 15.32. Entity type

    <div class="paragraph">

    We can also refer to the type of an entity as an expression.
    This is mainly useful when dealing with entity inheritance hierarchies.
    The type can expressed using a `TYPE` function used to refer to the type of an identification variable representing an entity.
    The name of the entity also serves as a way to refer to an entity type.
    Additionally, the entity type can be parameterized, in which case the entity&#8217;s Java Class reference would be bound as the parameter value.

    </div>
    <div id="hql-entity-type-exp-example" class="exampleblock">
    <div class="title">Example 388. Entity type expression examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Payment&gt; payments = entityManager.createQuery(
        "select p " +
        "from Payment p " +
        "where type(p) = CreditCardPayment", Payment.class )
    .getResultList();
    List&lt;Payment&gt; payments = entityManager.createQuery(
        "select p " +
        "from Payment p " +
        "where type(p) = :type", Payment.class )
    .setParameter( "type", WireTransferPayment.class)
    .getResultList();`</pre>
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

    HQL also has a legacy form of referring to an entity type, though that legacy form is considered deprecated in favor of `TYPE`.
    The legacy form would have used `p.class` in the examples rather than `type(p)`.
    It is mentioned only for completeness.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">