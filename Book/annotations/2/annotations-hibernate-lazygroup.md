#### 24.2.45. `@LazyGroup`

<div class="paragraph">

The [`@LazyGroup`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/LazyGroup.html) annotation is used to specify that an entity attribute should be fetched along with all the other attributes belonging to the same group.

</div>
<div class="paragraph">

To load entity attributes lazily, bytecode enhancement is needed.
By default, all non-collection attributes are loaded in one group named "DEFAULT".

</div>
<div class="paragraph">

This annotation allows defining different groups of attributes to be initialized together when access one attribute in the group.

</div>
<div class="paragraph">

See the [`@LazyGroup` mapping](#BytecodeEnhancement-lazy-loading-example) section for more info.

</div>
</div>
<div class="sect3">

