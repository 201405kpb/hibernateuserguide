 ### 21.18. `@OneToMany` with `@JoinColumn`

    <div class="paragraph">

    When a collection is mapped using these two annotations, Hibernate doesn&#8217;t generate a join table.
    Envers, however, has to do this so that when you read the revisions in which the related entity has changed, you don&#8217;t get false results.

    </div>
    <div class="paragraph">

    To be able to name the additional join table, there is a special annotation: `@AuditJoinTable`, which has similar semantics to JPA `@JoinTable`.

    </div>
    <div class="paragraph">

    One special case are relations mapped with `@OneToMany` with `@JoinColumn` on the one side, and `@ManyToOne` and `@JoinColumn( insertable=false, updatable=false`) on the many side.
    Such relations are, in fact, bidirectional, but the owning side is the collection.

    </div>
    <div class="paragraph">

    To properly audit such relations with Envers, you can use the `@AuditMappedBy` annotation.
    It enables you to specify the reverse property (using the `mappedBy` element).
    In case of indexed collections, the index column must also be mapped in the referenced entity (using `@Column( insertable=false, updatable=false )`, and specified using `positionMappedBy`.
    This annotation will affect only the way Envers works.
    Please note that the annotation is experimental and may change in the future.

    </div>
    </div>
    <div class="sect2">