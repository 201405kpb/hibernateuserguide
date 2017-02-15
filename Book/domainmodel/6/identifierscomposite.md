#### 2.6.2. Composite identifiers

    <div class="paragraph">

    Composite identifiers correspond to one or more persistent attributes.
    Here are the rules governing composite identifiers, as defined by the JPA specification.

    </div>
    <div class="ulist">

*   The composite identifier must be represented by a "primary key class".
    The primary key class may be defined using the `javax.persistence.EmbeddedId` annotation (see [Composite identifiers with `@EmbeddedId`](#identifiers-composite-aggregated)),
    or defined using the `javax.persistence.IdClass` annotation (see [Composite identifiers with `@IdClass`](#identifiers-composite-nonaggregated)).
*   The primary key class must be public and must have a public no-arg constructor.
*   The primary key class must be serializable.
*   The primary key class must define equals and hashCode methods, consistent with equality for the underlying database types to which the primary key is mapped.
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The restriction that a composite identifier has to be represented by a "primary key class" is only JPA specific.
    Hibernate does allow composite identifiers to be defined without a "primary key class", although that modeling technique is deprecated and therefore omitted from this discussion.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The attributes making up the composition can be either basic, composite, ManyToOne.
    Note especially that collections and one-to-ones are never appropriate.

    </div>
    </div>
    <div class="sect3">