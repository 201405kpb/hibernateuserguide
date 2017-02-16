 ## 16. Criteria

    <div class="sectionbody">
    <div class="paragraph">

    Criteria queries offer a type-safe alternative to HQL, JPQL and native SQL queries.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Hibernate offers an older, legacy `org.hibernate.Criteria` API which should be considered deprecated.
    No feature development will target those APIs. Eventually, Hibernate-specific criteria features will be ported as extensions to the JPA `javax.persistence.criteria.CriteriaQuery`.
    For details on the `org.hibernate.Criteria` API, see [Legacy Hibernate Criteria Queries](#appendix-legacy-criteria).

    </div>
    <div class="paragraph">

    This chapter will focus on the JPA APIs for declaring type-safe criteria queries.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Criteria queries are a programmatic, type-safe way to express a query.
    They are type-safe in terms of using interfaces and classes to represent various structural parts of a query such as the query itself, the select clause, or an order-by, etc.
    They can also be type-safe in terms of referencing attributes as we will see in a bit.
    Users of the older Hibernate `org.hibernate.Criteria` query API will recognize the general approach, though we believe the JPA API to be superior as it represents a clean look at the lessons learned from that API.

    </div>
    <div class="paragraph">

    Criteria queries are essentially an object graph, where each part of the graph represents an increasing (as we navigate down this graph) more atomic part of query.
    The first step in performing a criteria query is building this graph.
    The `javax.persistence.criteria.CriteriaBuilder` interface is the first thing with which you need to become acquainted to begin using criteria queries.
    Its role is that of a factory for all the individual pieces of the criteria.
    You obtain a `javax.persistence.criteria.CriteriaBuilder` instance by calling the `getCriteriaBuilder()` method of either `javax.persistence.EntityManagerFactory` or `javax.persistence.EntityManager`.

    </div>
    <div class="paragraph">

    The next step is to obtain a `javax.persistence.criteria.CriteriaQuery`.
    This is accomplished using one of the three methods on `javax.persistence.criteria.CriteriaBuilder` for this purpose:

    </div>
    <div class="ulist">

*   `&lt;T&gt; CriteriaQuery&lt;T&gt; createQuery( Class&lt;T&gt; resultClass )`
*   `CriteriaQuery&lt;Tuple&gt; createTupleQuery()`
*   `CriteriaQuery&lt;Object&gt; createQuery()`
    </div>
    <div class="paragraph">

    Each serves a different purpose depending on the expected type of the query results.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Chapter 6 Criteria API of the JPA Specification already contains a decent amount of reference material pertaining to the various parts of a criteria query.
    So rather than duplicate all that content here, let&#8217;s instead look at some of the more widely anticipated usages of the API.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="sect2">