#### 12.3.2. HQL syntax for INSERT

    <div class="exampleblock">
    <div class="title">Example 309. Pseudo-syntax for INSERT statements</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO EntityName
    	properties_list
    SELECT properties_list
    FROM ...`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Only the `INSERT INTO &#8230;&#8203; SELECT &#8230;&#8203;` form is supported.
    You cannot specify explicit values to insert.

    </div>
    <div class="paragraph">

    The `properties_list` is analogous to the column specification in the `SQL` `INSERT` statement.
    For entities involved in mapped inheritance, you can only use properties directly defined on that given class-level in the `properties_list`.
    Superclass properties are not allowed and subclass properties are irrelevant.
    In other words, `INSERT` statements are inherently non-polymorphic.

    </div>
    <div class="paragraph">

    The SELECT statement can be any valid HQL select query, but the return types must match the types expected by the INSERT.
    Hibernate verifies the return types during query compilation, instead of expecting the database to check it.
    Problems might result from Hibernate types which are equivalent, rather than equal.
    One such example is a mismatch between a property defined as an `org.hibernate.type.DateType` and a property defined as an `org.hibernate.type.TimestampType`,
    even though the database may not make a distinction, or may be capable of handling the conversion.

    </div>
    <div class="paragraph">

    If id property is not specified in the `properties_list`,
    Hibernate generates a value automatically.
    Automatic generation is only available if you use ID generators which operate on the database.
    Otherwise, Hibernate throws an exception during parsing.
    Available in-database generators are `org.hibernate.id.SequenceGenerator` and its subclasses, and objects which implement `org.hibernate.id.PostInsertIdentifierGenerator`.
    The most notable exception is `org.hibernate.id.TableHiLoGenerator`, which does not expose a selectable way to get its values.

    </div>
    <div class="paragraph">

    For properties mapped as either version or timestamp, the insert statement gives you two options.
    You can either specify the property in the properties_list, in which case its value is taken from the corresponding select expressions, or omit it from the properties_list,
    in which case the seed value defined by the org.hibernate.type.VersionType is used.

    </div>
    <div id="batch-bulk-hql-insert-example" class="exampleblock">
    <div class="title">Example 310. HQL INSERT statement</div>
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
    <div class="paragraph">

    This section is only a brief overview of HQL. For more information, see [HQL](#hql).

    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">