 ### 20.6. Enterprise OSGi JPA Container
     <div class="paragraph">

    In order to utilize container-managed JPA, an Enterprise OSGi JPA container must be active in the runtime.
    In Karaf, this means Aries JPA, which is included out-of-the-box (simply activate the `jpa` and `transaction` features).
    Originally, we intended to include those dependencies within our own `features.xml`.
    However, after guidance from the Karaf and Aries teams, it was pulled out.
    This allows Hibernate OSGi to be portable and not be directly tied to Aries versions, instead having the user choose which to use.

    </div>
    <div class="paragraph">
    
    

    That being said, the QuickStart/Demo projects include a sample [features.xml](https://github.com/hibernate/hibernate-demos/tree/master/hibernate-orm/osgi/managed-jpa/features.xml)
    showing which features need activated in Karaf in order to support this environment.
    As mentioned, use this purely as a reference!

    </div>
    </div>
    <div class="sect2">