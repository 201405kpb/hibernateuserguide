#### 24.2.71. `@SelectBeforeUpdate`

<div class="paragraph">

The [`@SelectBeforeUpdate`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/SelectBeforeUpdate.html) annotation is used to specify that the current annotated entity state be selected from the database when determining whether to perform an update when the detached entity is reattached.

</div>
<div class="paragraph">

See the [`OptimisticLockType.DIRTY` mapping](#locking-optimistic-lock-type-dirty-example) section for more info on how `@SelectBeforeUpdate` works.

</div>
</div>
<div class="sect3">

