### 15.7. Select statements

    <div class="paragraph">

    The [BNF](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_Form) for `SELECT` statements in HQL is:

    </div>
    <div id="hql-select-bnf-example" class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`select_statement :: =
        [select_clause]
        from_clause
        [where_clause]
        [groupby_clause]
        [having_clause]
        [orderby_clause]`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The simplest possible HQL `SELECT` statement is of the form:

    </div>
    <div id="hql-select-simplest-example" class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = session.createQuery(
        "from Person" )
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The select statement in JPQL is exactly the same as for HQL except that JPQL requires a `select_clause`, whereas HQL does not.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    <div class="paragraph">

    Even though HQL does not require the presence of a `select_clause`, it is generally good practice to include one.
    For simple queries the intent is clear and so the intended result of the `select_clause` is east to infer.
    But on more complex queries that is not always the case.

    </div>
    <div class="paragraph">

    It is usually better to explicitly specify intent.
    Hibernate does not actually enforce that a `select_clause` be present even when parsing JPQL queries, however applications interested in JPA portability should take heed of this.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">