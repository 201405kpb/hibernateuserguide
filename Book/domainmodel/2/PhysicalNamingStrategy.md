#显示命名策略
   Many organizations define rules around the naming of database objects (tables, columns, foreign-keys, etc).
The idea of a PhysicalNamingStrategy is to help implement such naming rules without having to hard-code them into the mapping via explicit names.

While the purpose of an ImplicitNamingStrategy is to determine that an attribute named `accountNumber` maps to
a logical column name of `accountNumber` when not explicitly specified, the purpose of a PhysicalNamingStrategy
would be, for example, to say that the physical column name should instead be abbreviated `acct_num`.

It is true that the resolution to `acct_num` could have been handled in an ImplicitNamingStrategy in this case.
But the point is separation of concerns. The PhysicalNamingStrategy will be applied regardless of whether
the attribute explicitly specified the column name or whether we determined that implicitly. The
ImplicitNamingStrategy would only be applied if an explicit name was not given. So it depends on needs
and intent.


The default implementation is to simply use the logical name as the physical name. However
applications and integrations can define custom implementations of this PhysicalNamingStrategy
contract. Here is an example PhysicalNamingStrategy for a fictitious company named Acme Corp
whose naming standards are to:

* prefer underscore-delimited words rather than camel-casing
* replace certain words with standard abbreviations

Example 2. Example PhysicalNamingStrategy implementation
```java
    
    
```
There are multiple ways to specify the PhysicalNamingStrategy to use. First, applications can specify
the implementation using the `hibernate.physical_naming_strategy` configuration setting which accepts:


* reference to a Class that implements the `org.hibernate.boot.model.naming.PhysicalNamingStrategy` contract
* FQN of a class that implements the `org.hibernate.boot.model.naming.PhysicalNamingStrategy` contract


Secondly, applications and integrations can leverage `org.hibernate.boot.MetadataBuilder#applyPhysicalNamingStrategy`.
See [Bootstrap](#bootstrap) for additional details on bootstrapping.