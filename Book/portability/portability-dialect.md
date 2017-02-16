    ### 22.2. Dialect

    <div class="paragraph">

    The first line of portability for Hibernate is the dialect, which is a specialization of the `org.hibernate.dialect.Dialect` contract.
    A dialect encapsulates all the differences in how Hibernate must communicate with a particular database to accomplish some task like getting a sequence value or structuring a SELECT query.
    Hibernate bundles a wide range of dialects for many of the most popular databases.
    If you find that your particular database is not among them, it is not terribly difficult to write your own.

    </div>
    </div>
    <div class="sect2">