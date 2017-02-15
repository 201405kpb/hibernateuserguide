 #### 2.5.6. Mapping the entity

    <div class="paragraph">

    The main piece in mapping the entity is the `javax.persistence.Entity` annotation.
    The `@Entity` annotation defines just one attribute `name` which is used to give a specific entity name for use in JPQL queries.
    By default, the entity name represents the unqualified name of the entity class itself.

    </div>
    <div class="exampleblock">
    <div class="title">Example 86. Simple `@Entity`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Simple {
        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    An entity models a database table.
    The identifier uniquely identifies each row in that table.
    By default, the name of the table is assumed to be the same as the name of the entity.
    To explicitly give the name of the table or to specify other information about the table, we would use the `javax.persistence.Table` annotation.

    </div>
    <div class="exampleblock">
    <div class="title">Example 87. Simple `@Entity` with `@Table`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    @Table( catalog = "CRM", schema = "purchasing", name = "t_simple" )
    public class Simple {
        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">