### 15.19. Polymorphism

    <div class="paragraph">

    HQL and JPQL queries are inherently polymorphic.

    </div>
    <div id="hql-polymorphism-example" class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Payment&gt; payments = entityManager.createQuery(
        "select p " +
        "from Payment p ", Payment.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This query names the `Payment` entity explicitly.
    However, all subclasses of `Payment` are also available to the query.
    So if the `CreditCardPayment` and `WireTransferPayment` entities extend the `Payment` class, all three types would be available to the entity query,
    and the query would return instances of all three.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    This can be altered by using either the `org.hibernate.annotations.Polymorphism` annotation (global, and Hibernate-specific) or limiting them using in the query itself using an entity type expression.

    </div>
    <div class="paragraph">

    The HQL query `from java.lang.Object` is totally valid! It returns every object of every type defined in your application.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">