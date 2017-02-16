 ### 15.9. Delete statements

    <div class="paragraph">

    The BNF for `DELETE` statements is the same in HQL and JPQL:

    </div>
    <div id="hql-delete-bnf-example" class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`delete_statement ::=
        delete_clause [where_clause]

    delete_clause ::=
        DELETE FROM entity_name [[AS] identification_variable]`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    A `DELETE` statement is also executed using the `executeUpdate()` method of either `org.hibernate.query.Query` or `javax.persistence.Query`.

    </div>
    </div>
    <div class="sect2">