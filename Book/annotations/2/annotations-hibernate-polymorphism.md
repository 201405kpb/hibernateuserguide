#### 24.2.68. `@Polymorphism`

<div class="paragraph">

The [`@Polymorphism`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/Polymorphism.html) annotation is used to define the [`PolymorphismType`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/PolymorphismType.html) Hibernate will apply to entity hierarchies.

</div>
<div class="paragraph">

There are two possible `PolymorphismType` options:

</div>
<div class="dlist">
<dl>
<dt class="hdlist1">EXPLICIT</dt>
<dd>

The current annotated entity is retrieved only if explicitly asked.

</dd>
<dt class="hdlist1">IMPLICIT</dt>
<dd>

The current annotated entity is retrieved if any of its super entity are retrieved. This is the default option.

</dd>
</dl>
</div>
</div>
<div class="sect3">

