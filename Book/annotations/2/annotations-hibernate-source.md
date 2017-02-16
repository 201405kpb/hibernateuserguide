#### 24.2.75. `@Source`

<div class="paragraph">

The [`@Source`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/Source.html) annotation is used in conjunction with a `@Version` timestamp entity attribute indicating
the [`SourceType`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/SourceType.html) of the timestamp value.

</div>
<div class="paragraph">

The `SourceType` offers two options:

</div>
<div class="dlist">
<dl>
<dt class="hdlist1">DB</dt>
<dd>

Get the timestamp from the database.

</dd>
<dt class="hdlist1">VM</dt>
<dd>

Get the timestamp from the current JVM.

</dd>
</dl>
</div>
</div>
<div class="sect3">

