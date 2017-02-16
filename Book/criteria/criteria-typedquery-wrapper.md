### 16.5. Selecting a wrapper

    <div class="paragraph">

    Another alternative to [Selecting multiple values](#criteria-typedquery-multiselect) is to instead select an object that will "wrap" the multiple values.
    Going back to the example query there, rather than returning an array of _[Person#id, Person#nickName]_, instead declare a class that holds these values and use that as a return object.

    </div>
    <div id="criteria-typedquery-wrapper-example" class="exampleblock">
    <div class="title">Example 413. Selecting a wrapper</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public class PersonWrapper {

        private final Long id;

        private final String nickName;

        public PersonWrapper(Long id, String nickName) {
            this.id = id;
            this.nickName = nickName;
        }

        public Long getId() {
            return id;
        }

        public String getNickName() {
            return nickName;
        }
    }

    CriteriaBuilder builder = entityManager.getCriteriaBuilder();

    CriteriaQuery&lt;PersonWrapper&gt; criteria = builder.createQuery( PersonWrapper.class );
    Root&lt;Person&gt; root = criteria.from( Person.class );

    Path&lt;Long&gt; idPath = root.get( Person_.id );
    Path&lt;String&gt; nickNamePath = root.get( Person_.nickName);

    criteria.select( builder.construct( PersonWrapper.class, idPath, nickNamePath ) );
    criteria.where( builder.equal( root.get( Person_.name ), "John Doe" ) );

    List&lt;PersonWrapper&gt; wrappers = entityManager.createQuery( criteria ).getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    First, we see the simple definition of the wrapper object we will be using to wrap our result values.
    Specifically, notice the constructor and its argument types.
    Since we will be returning `PersonWrapper` objects, we use `PersonWrapper` as the type of our criteria query.

    </div>
    <div class="paragraph">

    This example illustrates the use of the `javax.persistence.criteria.CriteriaBuilder` method construct which is used to build a wrapper expression.
    For every row in the result we are saying we would like a `PersonWrapper` instantiated with the remaining arguments by the matching constructor.
    This wrapper expression is then passed as the select.

    </div>
    </div>
    <div class="sect2">