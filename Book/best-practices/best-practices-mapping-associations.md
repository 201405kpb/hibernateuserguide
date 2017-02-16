 #### 25.4.2. Associations

    <div class="paragraph">

    JPA offers four entity association types:

    </div>
    <div class="ulist">

*   `@ManyToOne`
*   `@OneToOne`
*   `@OneToMany`
*   `@ManyToMany`
    </div>
    <div class="paragraph">

    And an `@ElementCollection` for collections of embeddables.

    </div>
    <div class="paragraph">

    Because object associations can be bidirectional, there are many possible combinations of associations.
    However, not every possible association type is efficient from a database perspective.

    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The closer the association mapping is to the underlying database relationship, the better it will perform.

    </div>
    <div class="paragraph">

    On the other hand, the more exotic the association mapping, the better the chance of being inefficient.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Therefore, the `@ManyToOne` and the `@OneToOne` child-side association are best to represent a `FOREIGN KEY` relationship.

    </div>
    <div class="paragraph">

    The parent-side `@OneToOne` association requires bytecode enhancement
    so that the association can be loaded lazily. Otherwise, the parent-side is always fetched even if the association is marked with `FetchType.LAZY`.

    </div>
    <div class="paragraph">

    For this reason, it&#8217;s best to map `@OneToOne` association using `@MapsId` so that the `PRIMARY KEY` is shared between the child and the parent entities.
    When using `@MapsId`, the parent-side becomes redundant since the child-entity can be easily fetched using the parent entity identifier.

    </div>
    <div class="paragraph">

    For collections, the association can be either:

    </div>
    <div class="ulist">

*   unidirectional
*   bidirectional
    </div>
    <div class="paragraph">

    For unidirectional collections, `Sets` are the best choice because they generate the most efficient SQL statements.
    Unidirectional `Lists` are less efficient than a `@ManyToOne` association.

    </div>
    <div class="paragraph">

    Bidirectional associations are usually a better choice because the `@ManyToOne` side controls the association.

    </div>
    <div class="paragraph">

    Embeddable collections (``@ElementCollection`) are unidirectional associations, hence `Sets` are the most efficient, followed by ordered `Lists`, whereas bags (unordered `Lists`) are the least efficient.

    </div>
    <div class="paragraph">

    The `@ManyToMany` annotation is rarely a good choice because it treats both sides as unidirectional associations.

    </div>
    <div class="paragraph">

    For this reason, it&#8217;s much better to map the link table as depicted in the [Bidirectional many-to-many with link entity lifecycle](#associations-many-to-many-bidirectional-with-link-entity-lifecycle-example) section.
    Each `FOREIGN KEY` column will be mapped as a `@ManyToOne` association.
    On each parent-side, a bidirectional `@OneToMany` association is going to map to the aforementioned `@ManyToOne` relationship in the link entity.

    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Just because you have support for collections, it does not mean that you have to turn any one-to-many database relationship into a collection.

    </div>
    <div class="paragraph">

    Sometimes, a `@ManyToOne` association is sufficient, and the collection can be simply replaced by an entity query which is easier to paginate or filter.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    <div class="sect2">