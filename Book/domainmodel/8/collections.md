 ### 2.8. Collections

    <div class="paragraph">

    Naturally Hibernate also allows to persist collections.
    These persistent collections can contain almost any other Hibernate type, including: basic types, custom types, components and references to other entities.
    In this context, the distinction between value and reference semantics is very important.
    An object in a collection might be handled with _value_ semantics (its life cycle being fully depends on the collection owner),
    or it might be a reference to another entity with its own life cycle.
    In the latter case, only the _link_ between the two objects is considered to be a state held by the collection.

    </div>
    <div class="paragraph">

    The owner of the collection is always an entity, even if the collection is defined by an embeddable type.
    Collections form one/many-to-many associations between types so there can be:

    </div>
    <div class="ulist">

*   value type collections
*   embeddable type collections
*   entity collections
    </div>
    <div class="paragraph">

    Hibernate uses its own collection implementations which are enriched with lazy-loading, caching or state change detection semantics.
    For this reason, persistent collections must be declared as an interface type.
    The actual interface might be `java.util.Collection`, `java.util.List`, `java.util.Set`, `java.util.Map`, `java.util.SortedSet`, `java.util.SortedMap` or even other object types (meaning you will have to write an implementation of `org.hibernate.usertype.UserCollectionType`).

    </div>
    <div class="paragraph">

    As the following example demonstrates, it&#8217;s important to use the interface type and not the collection implementation, as declared in the entity mapping.

    </div>
    <div id="collections-collection-proxy-example" class="exampleblock">
    <div class="title">Example 142. Hibernate uses its own collection implementations</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;

        @ElementCollection
        private List&lt;String&gt; phones = new ArrayList&lt;&gt;();

        public List&lt;String&gt; getPhones() {
            return phones;
        }
    }

    Person person = entityManager.find( Person.class, 1L );
    //Throws java.lang.ClassCastException: org.hibernate.collection.internal.PersistentBag cannot be cast to java.util.ArrayList
    ArrayList&lt;String&gt; phones = (ArrayList&lt;String&gt;) person.getPhones();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    It is important that collections be defined using the appropriate Java Collections Framework interface rather than a specific implementation.
    From a theoretical perspective, this just follows good design principles.
    From a practical perspective, Hibernate (like other persistence providers) will use their own collection implementations which conform to the Java Collections Framework interfaces.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The persistent collections injected by Hibernate behave like `ArrayList`, `HashSet`, `TreeSet`, `HashMap` or `TreeMap`, depending on the interface type.

    </div>
    <div class="sect3">
