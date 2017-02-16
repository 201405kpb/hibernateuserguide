#### 24.2.59. `@NotFound`

<div class="paragraph">

The [`@NotFound`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/NotFound.html) annotation is used to specify the [`NotFoundAction`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/NotFoundAction.html) strategy for when an element is not found in a given association.

</div>
<div class="paragraph">

The `NotFoundAction` defines with two possibilities:

</div>
<div class="dlist">
<dl>
<dt class="hdlist1">`EXCEPTION`</dt>
<dd>

An exception is thrown when an element is not found (default and recommended).

</dd>
<dt class="hdlist1">`IGNORE`</dt>
<dd>

Ignore the element when not found in the database.

</dd>
</dl>
</div>
</div>
<div class="sect3">

