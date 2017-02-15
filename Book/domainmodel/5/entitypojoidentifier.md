 #### 2.5.5. Provide identifier attribute(s)

    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Historically this was considered optional.
    However, not defining identifier attribute(s) on the entity should be considered a deprecated feature that will be removed in an upcoming release.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The identifier attribute does not necessarily need to be mapped to the column(s) that physically define the primary key.
    However, it should map to column(s) that can uniquely identify each row.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    We recommend that you declare consistently-named identifier attributes on persistent classes and that you use a nullable (i.e., non-primitive) type.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The placement of the `@Id` annotation marks the [persistence state access strategy](#access).

    </div>
    <div class="exampleblock">
    <div class="title">Example 85. Identifier</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Id
    private Integer id;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Hibernate offers multiple identifier generation strategies, see the [Identifier Generators](#identifiers) chapter for more about this topic.

    </div>
    </div>
    <div class="sect3">