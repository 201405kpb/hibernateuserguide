 #### 2.6.6. Generated identifier values

    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    For discussion of generated values for non-identifier attributes, see [Generated properties](#mapping-generated)

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Hibernate supports identifier value generation across a number of different types.
    Remember that JPA portably defines identifier value generation just for integer types.

    </div>
    <div class="paragraph">

    Identifier value generation is indicates using the `javax.persistence.GeneratedValue` annotation.
    The most important piece of information here is the specified `javax.persistence.GenerationType` which indicates how values will be generated.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The discussions below assume that the application is using Hibernate&#8217;s "new generator mappings" as indicated by the `hibernate.id.new_generator_mappings` setting or
    `MetadataBuilder.enableNewIdentifierGeneratorSupport` method during bootstrap.
    Starting with Hibernate 5, this is set to true by default.
    If applications set this to false the resolutions discussed here will be very different.
    The rest of the discussion here assumes this setting is enabled (true).

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`AUTO` (the default)</dt>
    <dd>

    Indicates that the persistence provider (Hibernate) should choose an appropriate generation strategy. See [Interpreting AUTO](#identifiers-generators-auto).

    </dd>
    <dt class="hdlist1">`IDENTITY`</dt>
    <dd>

    Indicates that database IDENTITY columns will be used for primary key value generation. See [Using IDENTITY columns](#identifiers-generators-identity).

    </dd>
    <dt class="hdlist1">`SEQUENCE`</dt>
    <dd>

    Indicates that database sequence should be used for obtaining primary key values. See [Using sequences](#identifiers-generators-sequence).

    </dd>
    <dt class="hdlist1">`TABLE`</dt>
    <dd>

    Indicates that a database table should be used for obtaining primary key values. See [Using identifier table](#identifiers-generators-table).

    </dd>
    </dl>
    </div>
    </div>
    <div class="sect3">
