 ### 21.20. Benefits of audit table partitioning

    <div class="paragraph">

    Because audit tables tend to grow indefinitely, they can quickly become really large.
    When the audit tables have grown to a certain limit (varying per RDBMS and/or operating system) it makes sense to start using table partitioning.
    SQL table partitioning offers a lot of advantages including, but certainly not limited to:

    </div>
    <div class="olist arabic">

1.  Improved query performance by selectively moving rows to various partitions (or even purging old rows)
2.  Faster data loads, index creation, etc.
    </div>
    </div>
    <div class="sect2">
