#### 24.2.62. `@OptimisticLocking`

<div class="paragraph">

The [`@OptimisticLocking`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/OptimisticLocking.html) annotation is used to specify the current annotated an entity optimistic locking strategy.

</div>
<div class="paragraph">

The four possible strategies are defined by the [`OptimisticLockType`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/OptimisticLockType.html) enumeration:

</div>
<div class="dlist">
<dl>
<dt class="hdlist1">NONE</dt>
<dd>

The implicit optimistic locking mechanism is disabled.

</dd>
<dt class="hdlist1">VERSION</dt>
<dd>

The implicit optimistic locking mechanism is using a dedicated version column.

</dd>
<dt class="hdlist1">ALL</dt>
<dd>

The implicit optimistic locking mechanism is using **all** attributes as part of an expanded WHERE clause restriction for the `UPDATE` and `DELETE` SQL statements.

</dd>
<dt class="hdlist1">DIRTY</dt>
<dd>

The implicit optimistic locking mechanism is using the **dirty** attributes (the attributes that were modified) as part of an expanded WHERE clause restriction for the `UPDATE` and `DELETE` SQL statements.

</dd>
</dl>
</div>
<div class="paragraph">

See the [Versionless optimistic locking](#entity-pojo-optlock-versionless) section for more info.

</div>
</div>
<div class="sect3">

