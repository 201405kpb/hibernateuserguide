 ### 20.10. Obtaining an EntityManger

    <div class="paragraph">

    The easiest, and most supported, method of obtaining an `EntityManager` utilizes OSGi&#8217;s `OSGI-INF/blueprint/blueprint.xml` in your bundle.
    The container takes the name of your persistence unit, then automatically injects an `EntityManager` instance into your given bean attribute.

    </div>
    <div class="exampleblock">
    <div class="title">Example 492. OSGI-INF/blueprint/blueprint.xml</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;?xml version="1.0" encoding="UTF-8"?&gt;
    &lt;blueprint xmlns:jpa="http://aries.apache.org/xmlns/jpa/v1.0.0"
               xmlns:tx="http://aries.apache.org/xmlns/transactions/v1.0.0"
               default-activation="eager"
               xmlns="http://www.osgi.org/xmlns/blueprint/v1.0.0"&gt;

        &lt;!-- This gets the container-managed EntityManager and injects it into the DataPointServiceImpl bean.
        Assumes DataPointServiceImpl has an "entityManager" field with a getter and setter. --&gt;
        &lt;bean id="dpService" class="org.hibernate.osgitest.DataPointServiceImpl"&gt;
            &lt;jpa:context unitname="managed-jpa" property="entityManager"/&gt;
            &lt;tx:transaction method="*" value="Required"/&gt;
        &lt;/bean&gt;

        &lt;service ref="dpService" interface="org.hibernate.osgitest.DataPointService"/&gt;

    &lt;/blueprint&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">