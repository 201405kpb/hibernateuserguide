 ### 25.5. Inheritance

    <div class="paragraph">

    JPA offers `SINGLE_TABLE`, `JOINED`, and `TABLE_PER_CLASS` to deal with inheritance mapping, and each of these strategies has advantages and disadvantages.

    </div>
    <div class="ulist">

*   `SINGLE_TABLE` performs the best in terms of executed SQL statements. However, you cannot use `NOT NULL` constraints on the column-level. You can still use triggers and rules to enforce such constraints, but it&#8217;s not as straightforward.
*   `JOINED` addresses the data integrity concerns because every subclass is associated with a different table.
    Polymorphic queries or ``@OneToMany` base class associations don&#8217;t perform very well with this strategy.
    However, polymorphic @ManyToOne` associations are fine, and they can provide a lot of value.
*   `TABLE_PER_CLASS` should be avoided since it does not render efficient SQL statements.
    </div>
    </div>
    <div class="sect2">