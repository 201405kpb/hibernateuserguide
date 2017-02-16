### 11.8. The `@Fetch` annotation mapping

    <div class="paragraph">

    Besides the `FetchType.LAZY` or `FetchType.EAGER` JPA annotations,
    you can also use the Hibernate-specific `@Fetch` annotation that accepts one of the following `FetchMode`s:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">SELECT</dt>
    <dd>

    Use a secondary select for each individual entity, collection, or join load.

    </dd>
    <dt class="hdlist1">JOIN</dt>
    <dd>

    Use an outer join to load the related entities, collections or joins.

    </dd>
    <dt class="hdlist1">SUBSELECT</dt>
    <dd>

    Available for collections only.  
    When accessing a non-initialized collection, this fetch mode will trigger loading all elements of all collections of the same role
    for all owners associated with the persistence context using a single secondary select.

    </dd>
    </dl>
    </div>
    </div>
    <div class="sect2">