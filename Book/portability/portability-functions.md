 ### 22.5. Database functions

    <div class="admonitionblock warning">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    This is an area in Hibernate in need of improvement.
    In terms of portability concerns, this function handling currently works pretty well in HQL, however, it is quite lacking in all other aspects.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    SQL functions can be referenced in many ways by users.
    However, not all databases support the same set of functions.
    Hibernate, provides a means of mapping a _logical_ function name to a delegate which knows how to render that particular function, perhaps even using a totally different physical function call.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Technically this function registration is handled through the `org.hibernate.dialect.function.SQLFunctionRegistry` class which is intended to allow users to provide custom function definitions without having to provide a custom dialect.
    This specific behavior is not fully completed as of yet.

    </div>
    <div class="paragraph">

    It is sort of implemented such that users can programmatically register functions with the `org.hibernate.cfg.Configuration` and those functions will be recognized for HQL.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">