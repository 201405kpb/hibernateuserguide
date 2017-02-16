### 29.8. Collections

    <div class="paragraph">

    When using criteria against collections, there are two distinct cases.
    One is if the collection contains entities (eg. `&lt;one-to-many/&gt;` or `&lt;many-to-many/&gt;`) or components (`&lt;composite-element/&gt;` ),
    and the second is if the collection contains scalar values (`&lt;element/&gt;`).
    In the first case, the syntax is as given above in the section [Associations](#criteria-associations) where we restrict the `kittens` collection.
    Essentially we create a `Criteria` object against the collection property and restrict the entity or component properties using that instance.

    </div>
    <div class="paragraph">

    For querying a collection of basic values, we still create the `Criteria` object against the collection,
    but to reference the value, we use the special property "elements".
    For an indexed collection, we can also reference the index property using the special property "indices".

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = session.createCriteria(Cat.class)
        .createCriteria("nickNames")
        .add(Restrictions.eq("elements", "BadBoy"))
        .list();`</pre>
    </div>
    </div>
    </div>
    <div class="sect2">