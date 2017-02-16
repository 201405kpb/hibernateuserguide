#### 24.2.46. `@LazyToOne`

<div class="paragraph">

The [`@LazyToOne`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/LazyToOne.html) annotation is used to specify the laziness options, represented by [`LazyToOneOption`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/LazyToOneOption.html), available for a `@OneToOne` or `@ManyToOne` association.

</div>
<div class="paragraph">

`LazyToOneOption` defines the following alternatives:

</div>
<div class="dlist">
<dl>
<dt class="hdlist1">FALSE</dt>
<dd>

Eagerly load the association. This one is not needed since the JPA `FetchType.EAGER` offers the same behavior.

</dd>
<dt class="hdlist1">NO_PROXY</dt>
<dd>

This option will fetch the association lazily while returning real entity object.

</dd>
<dt class="hdlist1">PROXY</dt>
<dd>

This option will fetch the association lazily while returning a proxy instead.

</dd>
</dl>
</div>
</div>
<div class="sect3">

