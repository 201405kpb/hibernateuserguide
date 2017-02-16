    ### 6.1. `AUTO` flush

    <div class="paragraph">

    By default, Hibernate uses the `AUTO` flush mode which triggers a flush in the following circumstances:

    </div>
    <div class="ulist">

*   prior to committing a `Transaction`
*   prior to executing a JPQL/HQL query that overlaps with the queued entity actions
*   before executing any native SQL query that has no registered synchronization
    </div>
    <div class="sect3">