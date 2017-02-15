 #### 2.8.12. Custom collection types

    <div class="paragraph">

    If you wish to use other collection types than `List`, `Set` or ``Map`, like `Queue` for instance,
    you have to use a custom collection type, as illustrated by the following example:

    </div>
    <div id="collections-custom-collection-mapping-example" class="exampleblock">
    <div class="title">Example 170. Custom collection mapping example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Person")
    public static class Person {

        @Id
        private Long id;

        @OneToMany(cascade = CascadeType.ALL)
        @CollectionType( type = "org.hibernate.userguide.collections.type.QueueType")
        private Collection&lt;Phone&gt; phones = new LinkedList&lt;&gt;();

        //Getters and setters are omitted for brevity

    }

    @Entity(name = "Phone")
    public static class Phone implements Comparable&lt;Phone&gt; {

        @Id
        private Long id;

        private String type;

        @NaturalId
        @Column(name = "`number`")
        private String number;

        //Getters and setters are omitted for brevity

    }

    public class QueueType implements UserCollectionType {

        @Override
        public PersistentCollection instantiate(
                SharedSessionContractImplementor session,
                CollectionPersister persister) throws HibernateException {
            return new PersistentQueue( session );
        }

        @Override
        public PersistentCollection wrap(
                SharedSessionContractImplementor session,
                Object collection) {
            return new PersistentQueue( session, (List) collection );
        }

        @Override
        public Iterator getElementsIterator(Object collection) {
            return ( (Queue) collection ).iterator();
        }

        @Override
        public boolean contains(Object collection, Object entity) {
            return ( (Queue) collection ).contains( entity );
        }

        @Override
        public Object indexOf(Object collection, Object entity) {
            int i = ( (List) collection ).indexOf( entity );
            return ( i &lt; 0 ) ? null : i;
        }

        @Override
        public Object replaceElements(
                Object original,
                Object target,
                CollectionPersister persister,
                Object owner,
                Map copyCache,
                SharedSessionContractImplementor session)
                throws HibernateException {
            Queue result = (Queue) target;
            result.clear();
            result.addAll( (Queue) original );
            return result;
        }

        @Override
        public Object instantiate(int anticipatedSize) {
            return new LinkedList&lt;&gt;();
        }

    }

    public class PersistentQueue extends PersistentBag implements Queue {

        public PersistentQueue(SharedSessionContractImplementor session) {
            super( session );
        }

        public PersistentQueue(SharedSessionContractImplementor session, List list) {
            super( session, list );
        }

        @Override
        public boolean offer(Object o) {
            return add(o);
        }

        @Override
        public Object remove() {
            return poll();
        }

        @Override
        public Object poll() {
            int size = size();
            if(size &gt; 0) {
                Object first = get(0);
                remove( 0 );
                return first;
            }
            throw new NoSuchElementException();
        }

        @Override
        public Object element() {
            return peek();
        }

        @Override
        public Object peek() {
            return size() &gt; 0 ? get( 0 ) : null;
        }
    }`</pre>
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

    The reason why the `Queue` interface is not used for the entity attribute is because Hibernate only allows the following types:

    </div>
    <div class="ulist">

*   `java.util.List`
*   `java.util.Set`
*   `java.util.Map`
*   `java.util.SortedSet`
*   `java.util.SortedMap`
    </div>
    <div class="paragraph">

    However, the custom collection type can still be customized as long as the base type is one of the aformentioned persistent types.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    This way, the `Phone` collection can be used as a `java.util.Queue`:

    </div>
    <div id="collections-custom-collection-example" class="exampleblock">
    <div class="title">Example 171. Custom collection example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = entityManager.find( Person.class, 1L );
    Queue&lt;Phone&gt; phones = person.getPhones();
    Phone head = phones.peek();
    assertSame(head, phones.poll());
    assertEquals( 1, phones.size() );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">
