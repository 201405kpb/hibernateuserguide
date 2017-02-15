 #### 2.8.3. Collections of entities

    <div class="paragraph">

    If value type collections can only form a one-to-many association between an owner entity and multiple basic or embeddable types,
    entity collections can represent both [@OneToMany](#associations-one-to-many) and [@ManyToMany](#associations-many-to-many) associations.

    </div>
    <div class="paragraph">

    From a relational database perspective, associations are defined by the foreign key side (the child-side).
    With value type collections, only the entity can control the association (the parent-side), but for a collection of entities, both sides of the association are managed by the persistence context.

    </div>
    <div class="paragraph">

    For this reason, entity collections can be devised into two main categories: unidirectional and bidirectional associations.
    Unidirectional associations are very similar to value type collections since only the parent side controls this relationship.
    Bidirectional associations are more tricky since, even if sides need to be in-sync at all times, only one side is responsible for managing the association.
    A bidirectional association has an _owning_ side and an _inverse (mappedBy)_ side.

    </div>
    <div class="paragraph">

    Another way of categorizing entity collections is by the underlying collection type, and so we can have:

    </div>
    <div class="ulist">

*   bags
*   indexed lists
*   sets
*   sorted sets
*   maps
*   sorted maps
*   arrays
    </div>
    <div class="paragraph">

    In the following sections, we will go through all these collection types and discuss both unidirectional and bidirectional associations.

    </div>
    </div>
    <div class="sect3">
