 ### 21.3. Additional mapping annotations

    <div class="paragraph">

    The name of the audit table can be set on a per-entity basis, using the `@AuditTable` annotation.
    It may be tedious to add this annotation to every audited entity, so if possible, it&#8217;s better to use a prefix/suffix.

    </div>
    <div class="paragraph">

    If you have a mapping with secondary tables, audit tables for them will be generated in the same way (by adding the prefix and suffix).
    If you wish to overwrite this behavior, you can use the `@SecondaryAuditTable` and `@SecondaryAuditTables` annotations.

    </div>
    <div class="paragraph">

    If you&#8217;d like to override auditing behavior of some fields/properties inherited from `@MappedSuperclass` or in an embedded component,
    you can apply the `@AuditOverride( s )` annotation on the subtype or usage site of the component.

    </div>
    <div class="paragraph">

    If you want to audit a relation mapped with `@OneToMany` and `@JoinColumn`,
    please see [Mapping exceptions](#envers-mappingexceptions) for a description of the additional `@AuditJoinTable` annotation that you&#8217;ll probably want to use.

    </div>
    <div class="paragraph">

    If you want to audit a relation, where the target entity is not audited (that is the case for example with dictionary-like entities, which don&#8217;t change and don&#8217;t have to be audited),
    just annotate it with `@Audited( targetAuditMode = RelationTargetAuditMode.NOT_AUDITED )`.
    Then, while reading historic versions of your entity, the relation will always point to the "current" related entity.
    By default Envers throws `javax.persistence.EntityNotFoundException` when "current" entity does not exist in the database.
    Apply `@NotFound( action = NotFoundAction.IGNORE )` annotation to silence the exception and assign null value instead.
    The hereby solution causes implicit eager loading of to-one relations.

    </div>
    <div class="paragraph">

    If you&#8217;d like to audit properties of a superclass of an entity, which are not explicitly audited (they don&#8217;t have the `@Audited` annotation on any properties or on the class),
    you can set the `@AuditOverride( forClass = SomeEntity.class, isAudited = true/false )` annotation.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The `@Audited` annotation also features an `auditParents` attribute but it&#8217;s now deprecated in favor of `@AuditOverride`,

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">