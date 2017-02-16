  ### 22.4. Identifier generation

    <div class="paragraph">

    When considering portability between databases, another important decision is selecting the identifier generation strategy you want to use.
    Originally Hibernate provided the _native_ generator for this purpose, which was intended to select between a _sequence_, _identity_, or _table_ strategy depending on the capability of the underlying database.

    </div>
    <div class="paragraph">

    However, an insidious implication of this approach comes about when targeting some databases which support _identity_ generation and some which do not.
    _identity_ generation relies on the SQL definition of an IDENTITY (or auto-increment) column to manage the identifier value.
    It is what is known as a _post-insert_ generation strategy because the insert must actually happen before we can know the identifier value.

    </div>
    <div class="paragraph">

    Because Hibernate relies on this identifier value to uniquely reference entities within a persistence context,
    it must then issue the insert immediately when the users requests that the entity be associated with the session (e.g. like via `save()` or `persist()`) , regardless of current transactional semantics.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Hibernate was changed slightly, once the implications of this were better understood, so now the insert could be delayed in cases where this is feasible.

    </div>
    <div class="paragraph">

    The underlying issue is that the actual semantics of the application itself changes in these cases.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Starting with version 3.2.3, Hibernate comes with a set of [enhanced](http://in.relation.to/2082.lace) identifier generators targeting portability in a much different way.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    There are specifically 2 bundled _enhanced_generators:

    </div>
    <div class="ulist">

*   `org.hibernate.id.enhanced.SequenceStyleGenerator`
*   `org.hibernate.id.enhanced.TableGenerator`
    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The idea behind these generators is to port the actual semantics of the identifier value generation to the different databases.
    For example, the `org.hibernate.id.enhanced.SequenceStyleGenerator` mimics the behavior of a sequence on databases which do not support sequences by using a table.

    </div>
    </div>
    <div class="sect2">
