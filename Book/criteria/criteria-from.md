### 16.7. FROM clause

    <div class="exampleblock">
    <div class="content">
    <div class="paragraph">

    A `CriteriaQuery` object defines a query over one or more entity, embeddable, or basic abstract schema types.
    The root objects of the query are entities, from which the other types are reached by navigation.

    </div>
    <div class="paragraph">

    — JPA Specification, section 6.5.2 Query Roots, pg 262

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

    All the individual parts of the FROM clause (roots, joins, paths) implement the `javax.persistence.criteria.From` interface.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">