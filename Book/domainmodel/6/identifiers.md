  ### 2.6. Identifiers

    <div class="paragraph">

    Identifiers model the primary key of an entity. They are used to uniquely identify each specific entity.

    </div>
    <div class="paragraph">

    Hibernate and JPA both make the following assumptions about the corresponding database column(s):

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`UNIQUE`</dt>
    <dd>

    The values must uniquely identify each row.

    </dd>
    <dt class="hdlist1">`NOT NULL`</dt>
    <dd>

    The values cannot be null. For composite ids, no part can
    be null.

    </dd>
    <dt class="hdlist1">`IMMUTABLE`</dt>
    <dd>

    The values, once inserted, can never be changed.
    This is more a general guide, than a hard-fast rule as opinions vary.
    JPA defines the behavior of changing the value of the identifier attribute to be undefined; Hibernate simply does not support that.
    In cases where the values for the PK you have chosen will be updated, Hibernate recommends mapping the mutable value as a natural id, and use a surrogate id for the PK.
    See [Natural Ids](#naturalid).

    </dd>
    </dl>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Technically the identifier does not have to map to the column(s) physically defined as the table primary key.
    They just need to map to column(s) that uniquely identify each row.
    However this documentation will continue to use the terms identifier and primary key interchangeably.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Every entity must define an identifier. For entity inheritance hierarchies, the identifier must be defined just on the entity that is the root of the hierarchy.

    </div>
    <div class="paragraph">

    An identifier might be simple (single value) or composite (multiple values).

    </div>
    <div class="sect3">