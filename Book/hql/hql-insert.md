### 15.10. Insert statements

    <div class="paragraph">

    HQL adds the ability to define `INSERT` statements as well.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    There is no JPQL equivalent to this.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The BNF for an HQL `INSERT` statement is:

    </div>
    <div id="hql-insert-bnf-example" class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`insert_statement ::=
        insert_clause select_statement

    insert_clause ::=
        INSERT INTO entity_name (attribute_list)

    attribute_list ::=
        state_field[, state_field ]*`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `attribute_list` is analogous to the `column specification` in the SQL `INSERT` statement.
    For entities involved in mapped inheritance, only attributes directly defined on the named entity can be used in the `attribute_list`.
    Superclass properties are not allowed and subclass properties do not make sense.
    In other words, `INSERT` statements are inherently non-polymorphic.

    </div>
    <div class="paragraph">

    `select_statement` can be any valid HQL select query, with the caveat that the return types must match the types expected by the insert.
    Currently, this is checked during query compilation rather than allowing the check to relegate to the database.
    This may cause problems between Hibernate Types which are _equivalent_ as opposed to _equal_.
    For example, this might cause lead to issues with mismatches between an attribute mapped as a `org.hibernate.type.DateType` and an attribute defined as a `org.hibernate.type.TimestampType`,
    even though the database might not make a distinction or might be able to handle the conversion.

    </div>
    <div class="paragraph">

    For the id attribute, the insert statement gives you two options.
    You can either explicitly specify the id property in the `attribute_list`, in which case its value is taken from the corresponding select expression, or omit it from the `attribute_list` in which case a generated value is used.
    This latter option is only available when using id generators that operate "in the database"; attempting to use this option with any "in memory" type generators will cause an exception during parsing.

    </div>
    <div class="paragraph">

    For optimistic locking attributes, the insert statement again gives you two options.
    You can either specify the attribute in the `attribute_list` in which case its value is taken from the corresponding select expressions, or omit it from the `attribute_list` in which case the `seed value` defined by the corresponding `org.hibernate.type.VersionType` is used.

    </div>
    <div id="hql-insert-example" class="exampleblock">
    <div class="title">Example 365. INSERT query statements</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`int insertedEntities = session.createQuery(
        "insert into Partner (id, name) " +
        "select p.id, p.name " +
        "from Person p ")
    .executeUpdate();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">