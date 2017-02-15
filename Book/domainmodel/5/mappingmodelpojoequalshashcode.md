
    #### 2.5.7. Implementing `equals()` and `hashCode()`

    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Much of the discussion in this section deals with the relation of an entity to a Hibernate Session, whether the entity is managed, transient or detached.
    If you are unfamiliar with these topics, they are explained in the [Persistence Context](#pc) chapter.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Whether to implement `equals()` and `hashCode()` methods in your domain model, let alone how to implement them, is a surprisingly tricky discussion when it comes to ORM.

    </div>
    <div class="paragraph">

    There is really just one absolute case: a class that acts as an identifier must implement equals/hashCode based on the id value(s).
    Generally, this is pertinent for user-defined classes used as composite identifiers.
    Beyond this one very specific use case and few others we will discuss below, you may want to consider not implementing equals/hashCode altogether.

    </div>
    <div class="paragraph">

    So what&#8217;s all the fuss? Normally, most Java objects provide a built-in `equals()` and `hashCode()` based on the object&#8217;s identity, so each new object will be different from all others.
    This is generally what you want in ordinary Java programming.
    Conceptually however this starts to break down when you start to think about the possibility of multiple instances of a class representing the same data.

    </div>
    <div class="paragraph">

    This is, in fact, exactly the case when dealing with data coming from a database.
    Every time we load a specific `Person` from the database we would naturally get a unique instance.
    Hibernate, however, works hard to make sure that does not happen within a given `Session`.
    In fact, Hibernate guarantees equivalence of persistent identity (database row) and Java identity inside a particular session scope.
    So if we ask a Hibernate `Session` to load that specific Person multiple times we will actually get back the same _instance_:

    </div>
    <div class="exampleblock">
    <div class="title">Example 88. Scope of identity</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session=...;

    Person p1 = session.get( Person.class,1 );
    Person p2 = session.get( Person.class,1 );

    // this evaluates to true
    assert p1==p2;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Consider another example using a persistent `java.util.Set`:

    </div>
    <div class="exampleblock">
    <div class="title">Example 89. Set usage with Session-scoped identity</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session=...;

    Club club = session.get( Club.class,1 );

    Person p1 = session.get( Person.class,1 );
    Person p2 = session.get( Person.class,1 );

    club.getMembers().add( p1 );
    club.getMembers().add( p2 );

    // this evaluates to true
    assert club.getMembers.size()==1;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    However, the semantic changes when we mix instances loaded from different Sessions:

    </div>
    <div class="exampleblock">
    <div class="title">Example 90. Mixed Sessions</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session1=...;
    Session session2=...;

    Person p1 = session1.get( Person.class,1 );
    Person p2 = session2.get( Person.class,1 );

    // this evaluates to false
    assert p1==p2;`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session1=...;
    Session session2=...;

    Club club = session1.get( Club.class,1 );

    Person p1 = session1.get( Person.class,1 );
    Person p2 = session2.get( Person.class,1 );

    club.getMembers().add( p1 );
    club.getMembers().add( p2 );

    // this evaluates to ... well it depends
    assert club.getMembers.size()==1;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Specifically the outcome in this last example will depend on whether the `Person` class implemented equals/hashCode, and, if so, how.

    </div>
    <div class="paragraph">

    Consider yet another case:

    </div>
    <div class="exampleblock">
    <div class="title">Example 91. Sets with transient entities</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session=...;

    Club club = session.get( Club.class,1 );

    Person p1 = new Person(...);
    Person p2 = new Person(...);

    club.getMembers().add( p1 );
    club.getMembers().add( p2 );

    // this evaluates to ... again, it depends
    assert club.getMembers.size()==1;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In cases where you will be dealing with entities outside of a Session (whether they be transient or detached), especially in cases where you will be using them in Java collections,
    you should consider implementing equals/hashCode.

    </div>
    <div class="paragraph">

    A common initial approach is to use the entity&#8217;s identifier attribute as the basis for equals/hashCode calculations:

    </div>
    <div class="exampleblock">
    <div class="title">Example 92. Naive equals/hashCode implementation</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Person {

        @Id
        @GeneratedValue
        private Integer id;

        @Override
        public int hashCode() {
            return Objects.hash( id );
        }

        @Override
        public boolean equals(Object o) {
            if ( this == o ) {
                return true;
            }
            if ( !( o instanceof Person ) ) {
                return false;
            }
            Person person = (Person) o;
            return Objects.equals( id, person.id );
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    It turns out that this still breaks when adding transient instance of `Person` to a set as we saw in the last example:

    </div>
    <div class="exampleblock">
    <div class="title">Example 93. Still trouble</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session=...;
    session.getTransaction().begin();

    Club club = session.get( Club.class,1 );

    Person p1 = new Person(...);
    Person p2 = new Person(...);

    club.getMembers().add( p1 );
    club.getMembers().add( p2 );

    session.getTransaction().commit();

    // will actually resolve to false!
    assert club.getMembers().contains( p1 );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The issue here is a conflict between _the use of generated identifier_, _the contract of `Set`_ and _the equals/hashCode implementations_.
    `Set` says that the equals/hashCode value for an object should not change while the object is part of the Set.
    But that is exactly what happened here because the equals/hasCode are based on the (generated) id, which was not set until the `session.getTransaction().commit()` call.

    </div>
    <div class="paragraph">

    Note that this is just a concern when using generated identifiers.
    If you are using assigned identifiers this will not be a problem, assuming the identifier value is assigned prior to adding to the `Set`.

    </div>
    <div class="paragraph">

    Another option is to force the identifier to be generated and set prior to adding to the `Set`:

    </div>
    <div class="exampleblock">
    <div class="title">Example 94. Forcing identifier generation</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session=...;
    session.getTransaction().begin();

    Club club = session.get( Club.class,1 );

    Person p1 = new Person(...);
    Person p2 = new Person(...);

    session.save( p1 );
    session.save( p2 );
    session.flush();

    club.getMembers().add( p1 );
    club.getMembers().add( p2 );

    session.getTransaction().commit();

    // will actually resolve to false!
    assert club.getMembers().contains( p1 );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    But this is often not feasible.

    </div>
    <div class="paragraph">

    The final approach is to use a "better" equals/hashCode implementation, making use of a natural-id or business-key.

    </div>
    <div class="exampleblock">
    <div class="title">Example 95. Better equals/hashCode with natural-id</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Person {

        @Id
        @GeneratedValue
        private Integer id;

        @NaturalId
        private String ssn;

        protected Person() {
            // Constructor for ORM
        }

        public Person( String ssn ) {
            // Constructor for app
            this.ssn = ssn;
        }

        @Override
        public int hashCode() {
            return Objects.hash( ssn );
        }

        @Override
        public boolean equals(Object o) {
            if ( this == o ) {
                return true;
            }
            if ( !( o instanceof Person ) ) {
                return false;
            }
            Person person = (Person) o;
            return Objects.equals( ssn, person.ssn );
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    As you can see the question of equals/hashCode is not trivial, nor is there a one-size-fits-all solution.

    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Although using a natural-id is best for `equals` and `hashCode`, sometimes you only have the entity identifier that provides a unique constraint.

    </div>
    <div class="paragraph">

    It&#8217;s possible to use the entity identifier for equality check, but it needs a workaround:

    </div>
    <div class="ulist">

*   you need to provide a constant value for `hashCode` so that the hash code value does not change before and after the entity is flushed.
*   you need to compare the entity identifier equality only for non-transient entities.
    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    For details on mapping the identifier, see the [Identifiers](#identifiers) chapter.

    </div>
    </div>
    <div class="sect3">