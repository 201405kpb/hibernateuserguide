#### 24.2.60. `@OnDelete`

<div class="paragraph">

The [`@OnDelete`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/OnDelete.html) annotation is used to specify the delete strategy employed by the current annotated collection, array or joined subclasses.
This annotation is used by the automated schema generation tool to generated the appropriate FOREIGN KEY DDL cascade directive.

</div>
<div class="paragraph">

The two possible strategies are defined by the [`OnDeleteAction`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/OnDeleteAction.html) enumeration:

</div>
<div class="dlist">
<dl>
<dt class="hdlist1">CASCADE</dt>
<dd>

Use the database FOREIGN KEY cascade capabilities.

</dd>
<dt class="hdlist1">NO_ACTION</dt>
<dd>

Take no action.

</dd>
</dl>
</div>
</div>
<div class="sect3">

