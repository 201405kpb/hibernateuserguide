### 13.3. Entity inheritance and second-level cache mapping

    <div class="paragraph">

    When using inheritance, the JPA `@Cacheable` and the Hibernate-specific `@Cache` annotations should be declared at the root-entity level only.
    That being said, it is not possible to customize the base class `@Cacheable` or `@Cache` definition in subclasses.

    </div>
    <div class="paragraph">

    Although the JPA 2.1 specification says that the `@Cacheable` annotation could be overwritten by a subclass:

    </div>
    <div class="quoteblock">
    > <div class="paragraph">
> 
>     The value of the `Cacheable` annotation is inherited by subclasses; it can be
>     overridden by specifying `Cacheable` on a subclass.
> 
>     </div>
    <div class="attribution">
    &#8212; Section 11.1.7 of the JPA 2.1 Specification
    </div>
    </div>
    <div class="paragraph">

    Hibernate requires that a given entity hierarchy share the same caching semantics.

    </div>
    <div class="paragraph">

    The reasons why Hibernate requires that all entities belonging to an inheritance tree share the same caching definition can be summed as follows:

    </div>
    <div class="ulist">

*   from a performance perspective, adding an additional check on a per entity type level would slow the bootstrap process.
*   providing different caching semantics for subclasses would violate the [Liskov substitution principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle).
    <div class="paragraph">
    Assuming we have a base class, `Payment` and a subclass `CreditCardPayment`.
    If the `Payment` is not cacheable and the `CreditCardPayment` is cached, what should happen when executing the following code snippet:
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Payment payment = entityManager.find(Payment.class, creditCardPaymentId);
    CreditCardPayment creditCardPayment = (CreditCardPayment) CreditCardPayment;`</pre>
    </div>
    </div>
    <div class="paragraph">
    In this particular case, the second level cache key is formed of the entity class name and the identifier:
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`keyToLoad = {org.hibernate.engine.spi.EntityKey@4712}
     identifier = {java.lang.Long@4716} "2"
     persister = {org.hibernate.persister.entity.SingleTableEntityPersister@4629}
      "SingleTableEntityPersister(org.hibernate.userguide.model.Payment)"`</pre>
    </div>
    </div>
    <div class="paragraph">
    Should Hibernate load the `CreditCardPayment` from the cache as indicated by the actual entity type, or it should not use the cache since the `Payment` is not supposed to be cached?
    </div>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Because of all these intricacies, Hibernate only considers the base class `@Cacheable` and `@Cache` definition.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">