#### 24.2.70. `@RowId`

<div class="paragraph">

The [`@RowId`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/RowId.html) annotation is used to specify the database column used as a `ROWID` pseudocolumn.
For instance, Oracle defines the [`ROWID` pseudocolumn](https://docs.oracle.com/cd/B19306_01/server.102/b14200/pseudocolumns008.htm) which provides the address of every table row.

</div>
<div class="paragraph">

According to Oracle documentation, `ROWID` is the fastest way to access a single row from a table.

</div>
</div>
<div class="sect3">

