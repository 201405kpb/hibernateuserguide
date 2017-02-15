  ### 2.9. Natural Ids

    <div class="paragraph">

    Natural ids represent domain model unique identifiers that have a meaning in the real world too.
    Even if a natural id does not make a good primary key (surrogate keys being usually preferred), it&#8217;s still useful to tell Hibernate about it.
    As we will see later, Hibernate provides a dedicated, efficient API for loading an entity by its natural id much like it offers for loading by its identifier (PK).

    </div>
    <div class="sect3">