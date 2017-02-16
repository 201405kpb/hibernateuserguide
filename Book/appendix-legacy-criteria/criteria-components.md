### 29.7. Components

    <div class="paragraph">

    To add a restriction against a property of an embedded component, the component property name should be prepended to the property name when creating the `Restriction`.
    The criteria object should be created on the owning entity, and cannot be created on the component itself.
    For example, suppose the `Cat` has a component property `fullName` with sub-properties `firstName` and `lastName`:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = session.createCriteria(Cat.class)
        .add(Restrictions.eq("fullName.lastName", "Cattington"))
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    Note: this does not apply when querying collections of components, for that see below [Collections](#criteria-collections)

    </div>
    </div>
    <div class="sect2">