#### 2.8.6. Sets

<div class="paragraph">

Sets are collections that don&#8217;t allow duplicate entries and Hibernate supports both the unordered `Set` and the natural-ordering `SortedSet`.

</div>
<div class="sect4">

##### Unidirectional sets

<div class="paragraph">

The unidirectional set uses a link table to hold the parent-child associations and the entity mapping looks as follows:

</div>
<div id="collections-unidirectional-set-example" class="exampleblock">
<div class="title">Example 158. Unidirectional set</div>
<div class="content">
<div class="listingblock">
<div class="content">
<pre class="prettyprint highlight">`@Entity(name = "Person")
public static class Person {

@Id
private Long id;
@OneToMany(cascade = CascadeType.ALL)
private Set&lt;Phone&gt; phones = new HashSet&lt;&gt;();

public Person() {
}

public Person(Long id) {
this.id = id;
}

public Set&lt;Phone&gt; getPhones() {
return phones;
}
}

@Entity(name = "Phone")
public static class Phone {

@Id
private Long id;

private String type;

@NaturalId
@Column(name = "`number`")
private String number;

public Phone() {
}

public Phone(Long id, String type, String number) {
this.id = id;
this.type = type;
this.number = number;
}

public Long getId() {
return id;
}

public String getType() {
return type;
}

public String getNumber() {
return number;
}

@Override
public boolean equals(Object o) {
if ( this == o ) {
return true;
}
if ( o == null || getClass() != o.getClass() ) {
return false;
}
Phone phone = (Phone) o;
return Objects.equals( number, phone.number );
}

@Override
public int hashCode() {
return Objects.hash( number );
}
}`</pre>
</div>
</div>
</div>
</div>
<div class="paragraph">

The unidirectional set lifecycle is similar to that of the [Unidirectional bags](#collections-unidirectional-bag), so it can be omitted.
The only difference is that `Set` doesn&#8217;t allow duplicates, but this constraint is enforced by the Java object contract rather then the database mapping.

</div>
<div class="admonitionblock note">
<table>
<tr>
<td class="icon">

</td>
<td class="content">
<div class="paragraph">

When using sets, it&#8217;s very important to supply proper equals/hashCode implementations for child entities.
In the absence of a custom equals/hashCode implementation logic, Hibernate will use the default Java reference-based object equality which might render unexpected results when mixing detached and managed object instances.

</div>
</td>
</tr>
</table>
</div>
</div>
<div class="sect4">

##### Bidirectional sets

<div class="paragraph">

Just like bidirectional bags, the bidirectional set doesn&#8217;t use a link table, and the child table has a foreign key referencing the parent table primary key.
The lifecycle is just like with bidirectional bags except for the duplicates which are filtered out.

</div>
<div id="collections-bidirectional-set-example" class="exampleblock">
<div class="title">Example 159. Bidirectional set</div>
<div class="content">
<div class="listingblock">
<div class="content">
<pre class="prettyprint highlight">`@Entity(name = "Person")
public static class Person {

@Id
private Long id;

@OneToMany(mappedBy = "person", cascade = CascadeType.ALL)
private Set&lt;Phone&gt; phones = new HashSet&lt;&gt;();

public Person() {
}

public Person(Long id) {
this.id = id;
}

public Set&lt;Phone&gt; getPhones() {
return phones;
}

public void addPhone(Phone phone) {
phones.add( phone );
phone.setPerson( this );
}

public void removePhone(Phone phone) {
phones.remove( phone );
phone.setPerson( null );
}
}

@Entity(name = "Phone")
public static class Phone {

@Id
private Long id;

private String type;

@Column(name = "`number`", unique = true)
@NaturalId
private String number;

@ManyToOne
private Person person;

public Phone() {
}

public Phone(Long id, String type, String number) {
this.id = id;
this.type = type;
this.number = number;
}

public Long getId() {
return id;
}

public String getType() {
return type;
}

public String getNumber() {
return number;
}

public Person getPerson() {
return person;
}

public void setPerson(Person person) {
this.person = person;
}

@Override
public boolean equals(Object o) {
if ( this == o ) {
return true;
}
if ( o == null || getClass() != o.getClass() ) {
return false;
}
Phone phone = (Phone) o;
return Objects.equals( number, phone.number );
}

@Override
public int hashCode() {
return Objects.hash( number );
}
}`</pre>
</div>
</div>
</div>
</div>
</div>
</div>
<div class="sect3">