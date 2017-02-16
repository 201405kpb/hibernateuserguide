#### 25.6.1. Fetching associations

    <div class="paragraph">

    Related to associations, there are two major fetch strategies:

    </div>
    <div class="ulist">

*   `EAGER`
*   `LAZY`
    </div>
    <div class="paragraph">

    `EAGER` fetching is almost always a bad choice.

    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Prior to JPA, Hibernate used to have all associations as `LAZY` by default.
    However, when JPA 1.0 specification emerged, it was thought that not all providers would use Proxies. Hence, the `@ManyToOne` and the `@OneToOne` associations are now `EAGER` by default.

    </div>
    <div class="paragraph">

    The `EAGER` fetching strategy cannot be overwritten on a per query basis, so the association is always going to be retrieved even if you don&#8217;t need it.
    More, if you forget to `JOIN FETCH` an `EAGER` association in a JPQL query, Hibernate will initialize it with a secondary statement, which in turn can lead to N+1 query issues.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    So, `EAGER` fetching is to be avoided. For this reason, it&#8217;s better if all associations are marked as `LAZY` by default.

    </div>
    <div class="paragraph">

    However, `LAZY` associations must be initialized prior to being accessed. Otherwise, a `LazyInitializationException` is thrown.
    There are good and bad ways to treat the `LazyInitializationException`.

    </div>
    <div class="paragraph">

    The best way to deal with `LazyInitializationException` is to fetch all the required associations prior to closing the Persistence Context.
    The `JOIN FETCH` directive is good for `@ManyToOne` and `OneToOne` associations, and for at most one collection (e.g. `@OneToMany` or `@ManyToMany`).
    If you need to fetch multiple collections, to avoid a Cartesian Product, you should use secondary queries which are triggered either by navigating the `LAZY` association or by calling `Hibernate#initialize(proxy)` method.

    </div>
    </div>
    </div>
    <div class="sect2">