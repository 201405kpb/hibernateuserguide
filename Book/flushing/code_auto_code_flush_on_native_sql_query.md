 #### 6.1.3. `AUTO` flush on native SQL query

    <div class="paragraph">

    When executing a native SQL query, a flush is always triggered when using the `EntityManager` API.

    </div>
    <div id="flushing-auto-flush-sql-example" class="exampleblock">
    <div class="title">Example 265. Automatic flushing on native SQL using `EntityManager`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`assertTrue(((Number) entityManager
            .createNativeQuery( "select count(*) from Person")
            .getSingleResult()).intValue() == 0 );

    Person person = new Person( "John Doe" );
    entityManager.persist( person );

    assertTrue(((Number) entityManager
            .createNativeQuery( "select count(*) from Person")
            .getSingleResult()).intValue() == 1 );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `Session` API  doesn&#8217;t trigger an `AUTO` flush when executing a native query

    </div>
    <div id="flushing-auto-flush-sql-native-example" class="exampleblock">
    <div class="title">Example 266. Automatic flushing on native SQL using `Session`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`			assertTrue(((Number) entityManager
    					.createNativeQuery( "select count(*) from Person")
    					.getSingleResult()).intValue() == 0 );

    			Person person = new Person( "John Doe" );
    			entityManager.persist( person );
    			Session session = entityManager.unwrap(Session.class);

    			// for this to work, the Session/EntityManager must be put into COMMIT FlushMode
    			//  - this is a change since 5.2 to account for merging EntityManager functionality
    			// 		directly into Session.  Flushing would be the JPA-spec compliant behavior,
    			//		so we know do that by default.
    			session.setFlushMode( FlushModeType.COMMIT );
    			//		or using Hibernate's FlushMode enum
    			//session.setHibernateFlushMode( FlushMode.COMMIT );

    			assertTrue(((Number) session
    					.createSQLQuery( "select count(*) from Person")
    					.uniqueResult()).intValue() == 0 );
    			//end::flushing-auto-flush-sql-native-example[\]
    		} );
    	}

    	@Test
    	public void testFlushAutoSQLSynchronization() {
    		doInJPA( this::entityManagerFactory, entityManager -&gt; {
    			entityManager.createNativeQuery( "delete from Person" ).executeUpdate();;
    		} );
    		doInJPA( this::entityManagerFactory, entityManager -&gt; {
    			log.info( "testFlushAutoSQLSynchronization" );
    			assertTrue(((Number) entityManager
    					.createNativeQuery( "select count(*) from Person")
    					.getSingleResult()).intValue() == 0 );

    			Person person = new Person( "John Doe" );
    			entityManager.persist( person );
    			Session session = entityManager.unwrap( Session.class );

    			assertTrue(((Number) session
    					.createSQLQuery( "select count(*) from Person")
    					.addSynchronizedEntityClass( Person.class )
    					.uniqueResult()).intValue() == 1 );
    		} );
    	}

    	@Entity(name = "Person")
    	public static class Person {

    		@Id
    		@GeneratedValue
    		private Long id;

    		private String name;

    		public Person() {}

    		public Person(String name) {
    			this.name = name;
    		}

    		public Long getId() {
    			return id;
    		}

    		public String getName() {
    			return name;
    		}

    	}

    	@Entity(name = "Advertisement")
    	public static class Advertisement {

    		@Id
    		@GeneratedValue
    		private Long id;

    		private String title;

    		public Long getId() {
    			return id;
    		}

    		public void setId(Long id) {
    			this.id = id;
    		}

    		public String getTitle() {
    			return title;
    		}

    		public void setTitle(String title) {
    			this.title = title;
    		}
    	}
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To flush the `Session`, the query must use a synchronization:

    </div>
    <div id="flushing-auto-flush-sql-synchronization-example" class="exampleblock">
    <div class="title">Example 267. Automatic flushing on native SQL with `Session` synchronization</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`assertTrue(((Number) entityManager
            .createNativeQuery( "select count(*) from Person")
            .getSingleResult()).intValue() == 0 );

    Person person = new Person( "John Doe" );
    entityManager.persist( person );
    Session session = entityManager.unwrap( Session.class );

    assertTrue(((Number) session
            .createSQLQuery( "select count(*) from Person")
            .addSynchronizedEntityClass( Person.class )
            .uniqueResult()).intValue() == 1 );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">