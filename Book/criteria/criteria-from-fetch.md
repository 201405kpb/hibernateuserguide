### 16.10. Fetches

    <div class="paragraph">

    Just like in HQL and JPQL, criteria queries can specify that associated data be fetched along with the owner.
    Fetches are created by the numerous overloaded fetch methods of the `javax.persistence.criteria.From` interface.

    </div>
    <div id="criteria-from-fetch-example" class="exampleblock">
    <div class="title">Example 419. Join fetch example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CriteriaBuilder builder = entityManager.getCriteriaBuilder();

    CriteriaQuery&lt;Phone&gt; criteria = builder.createQuery( Phone.class );
    Root&lt;Phone&gt; root = criteria.from( Phone.class );

    // Phone.person is a @ManyToOne
    Fetch&lt;Phone, Person&gt; personFetch = root.fetch( Phone_.person );
    // Person.addresses is an @ElementCollection
    Fetch&lt;Person, String&gt; addressesJoin = personFetch.fetch( Person_.addresses );

    criteria.where( builder.isNotEmpty( root.get( Phone_.calls ) ) );

    List&lt;Phone&gt; phones = entityManager.createQuery( criteria ).getResultList();`</pre>
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

    Technically speaking, embedded attributes are always fetched with their owner.
    However in order to define the fetching of _Phone#addresses_ we needed a `javax.persistence.criteria.Fetch` because element collections are `LAZY` by default.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">