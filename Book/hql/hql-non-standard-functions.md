 ### 15.30. Non-standardized functions

    <div class="paragraph">

    Hibernate Dialects can register additional functions known to be available for that particular database product.
    These functions are also available in HQL (and JPQL, though only when using Hibernate as the JPA provider obviously).
    However, they would only be available when using that database Dialect.
    Applications that aim for database portability should avoid using functions in this category.

    </div>
    <div class="paragraph">

    Application developers can also supply their own set of functions.
    This would usually represent either custom SQL functions or aliases for snippets of SQL.
    Such function declarations are made by using the `addSqlFunction()` method of `org.hibernate.cfg.Configuration`.

    </div>
    </div>
    <div class="sect2">
