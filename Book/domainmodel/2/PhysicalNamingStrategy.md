# 显示命名策略

Many organizations define rules around the naming of database objects \(tables, columns, foreign-keys, etc\).The idea of a PhysicalNamingStrategy is to help implement such naming rules without having to hard-code them into the mapping via explicit names.  
许多组织定义围绕数据库对象（表，列，外键等）命名的规则。 PhysicalNamingStrategy的想法是帮助实现这样的命名规则，而不必通过显式名称将它们硬编码到映射中。

While the purpose of an ImplicitNamingStrategy is to determine that an attribute named `accountNumber` maps toa logical column name of `accountNumber` when not explicitly specified, the purpose of a PhysicalNamingStrategywould be, for example, to say that the physical column name should instead be abbreviated `acct_num`.  
虽然ImplicitNamingStrategy的目的是在未明确指定时确定名为accountNumber的属性映射到accountNumber的逻辑列名称，但是PhysicalNamingStrategy的目的将是将物理列名称应缩写为acct\_num 。

> ![PhysicalNamingStrategy](/Book/images/org/hibernate/docbook/note.png)  
> It is true that the resolution to `acct_num` could have been handled in an ImplicitNamingStrategy in this case.  
> But the point is separation of concerns. The PhysicalNamingStrategy will be applied regardless of whether  
> the attribute explicitly specified the column name or whether we determined that implicitly. The  
> ImplicitNamingStrategy would only be applied if an explicit name was not given. So it depends on needs  
> and intent.  
> 事实上，在这种情况下，acct\_num的决议可以在ImplicitNamingStrategy中处理。 但关键是分离关注点。 无论属性是否明确指定列名称或是否隐式确定，都将应用PhysicalNamingStrategy。 ImplicitNamingStrategy只有在未给出显式名称的情况下才会应用。 所以它取决于需要和意图。

The default implementation is to simply use the logical name as the physical name. However  
applications and integrations can define custom implementations of this PhysicalNamingStrategy  
contract. Here is an example PhysicalNamingStrategy for a fictitious company named Acme Corp  
whose naming standards are to:  
默认实现是简单地使用逻辑名作为物理名称。 然而，集成应用程序可以自定义PhysicalNamingStrategy接口的实现。 下面是一个名为Acme Corp的虚拟公司的PhysicalNamingStrategy示例，其命名标准是：

* prefer underscore-delimited words rather than camel-casing
* 优选下划线分隔的词而不是骆驼壳
* replace certain words with standard abbreviations
* 用标准缩写代替某些单词

_**Example 2. Example PhysicalNamingStrategy implementation**_  
_**例2. 自定义PhysicalNamingStrategy实现实例**_

```java
/*
 * Hibernate, Relational Persistence for Idiomatic Java
 *
 * License: GNU Lesser General Public License (LGPL), version 2.1 or later.
 * See the lgpl.txt file in the root directory or <http://www.gnu.org/licenses/lgpl-2.1.html>.
 */
package org.hibernate.userguide.naming;

import java.util.LinkedList;
import java.util.List;
import java.util.Locale;
import java.util.Map;
import java.util.TreeMap;

import org.hibernate.boot.model.naming.Identifier;
import org.hibernate.boot.model.naming.PhysicalNamingStrategy;
import org.hibernate.engine.jdbc.env.spi.JdbcEnvironment;

import org.apache.commons.lang3.StringUtils;

/**
 * An example PhysicalNamingStrategy that implements database object naming standards
 * for our fictitious company Acme Corp.
 * <p/>
 * In general Acme Corp prefers underscore-delimited words rather than camel casing.
 * <p/>
 * Additionally standards call for the replacement of certain words with abbreviations.
 *
 * @author Steve Ebersole
 */
public class AcmeCorpPhysicalNamingStrategy implements PhysicalNamingStrategy {
    private static final Map<String,String> ABBREVIATIONS = buildAbbreviationMap();

    @Override
    public Identifier toPhysicalCatalogName(Identifier name, JdbcEnvironment jdbcEnvironment) {
        // Acme naming standards do not apply to catalog names
        return name;
    }

    @Override
    public Identifier toPhysicalSchemaName(Identifier name, JdbcEnvironment jdbcEnvironment) {
        // Acme naming standards do not apply to schema names
        return null;
    }

    @Override
    public Identifier toPhysicalTableName(Identifier name, JdbcEnvironment jdbcEnvironment) {
        final List<String> parts = splitAndReplace( name.getText() );
        return jdbcEnvironment.getIdentifierHelper().toIdentifier(
                join( parts ),
                name.isQuoted()
        );
    }

    @Override
    public Identifier toPhysicalSequenceName(Identifier name, JdbcEnvironment jdbcEnvironment) {
        final LinkedList<String> parts = splitAndReplace( name.getText() );
        // Acme Corp says all sequences should end with _seq
        if ( !"seq".equalsIgnoreCase( parts.getLast() ) ) {
            parts.add( "seq" );
        }
        return jdbcEnvironment.getIdentifierHelper().toIdentifier(
                join( parts ),
                name.isQuoted()
        );
    }

    @Override
    public Identifier toPhysicalColumnName(Identifier name, JdbcEnvironment jdbcEnvironment) {
        final List<String> parts = splitAndReplace( name.getText() );
        return jdbcEnvironment.getIdentifierHelper().toIdentifier(
                join( parts ),
                name.isQuoted()
        );
    }

    private static Map<String, String> buildAbbreviationMap() {
        TreeMap<String,String> abbreviationMap = new TreeMap<> ( String.CASE_INSENSITIVE_ORDER );
        abbreviationMap.put( "account", "acct" );
        abbreviationMap.put( "number", "num" );
        return abbreviationMap;
    }

    private LinkedList<String> splitAndReplace(String name) {
        LinkedList<String> result = new LinkedList<>();
        for ( String part : StringUtils.splitByCharacterTypeCamelCase( name ) ) {
            if ( part == null || part.trim().isEmpty() ) {
                // skip null and space
                continue;
            }
            part = applyAbbreviationReplacement( part );
            result.add( part.toLowerCase( Locale.ROOT ) );
        }
        return result;
    }

    private String applyAbbreviationReplacement(String word) {
        if ( ABBREVIATIONS.containsKey( word ) ) {
            return ABBREVIATIONS.get( word );
        }

        return word;
    }

    private String join(List<String> parts) {
        boolean firstPass = true;
        String separator = "";
        StringBuilder joined = new StringBuilder();
        for ( String part : parts ) {
            joined.append( separator ).append( part );
            if ( firstPass ) {
                firstPass = false;
                separator = "_";
            }
        }
        return joined.toString();
    }
}
```

There are multiple ways to specify the PhysicalNamingStrategy to use. First, applications can specify  
the implementation using the `hibernate.physical_naming_strategy` configuration setting which accepts:
有多种方法可以指定要使用的PhysicalNamingStrategy的接口类型。 首先，应用程序可以使用`hibernate.physical_naming_strategy`配置或设置PhysicalNamingStrategy的方式。

* reference to a Class that implements the `org.hibernate.boot.model.naming.PhysicalNamingStrategy` contract
* 引用实现org.hibernate.boot.model.naming.PhysicalNamingStrategy`接口的类

* FQN of a class that implements the `org.hibernate.boot.model.naming.PhysicalNamingStrategy` contract
* 引用实现org.hibernate.boot.model.naming.PhysicalNamingStrategy`接口的类

Secondly, applications and integrations can leverage `org.hibernate.boot.MetadataBuilder#applyPhysicalNamingStrategy`.  
See [Bootstrap](/Book/bootstrap/README.md) for additional details on bootstrapping.
其次，应用程序可以利用`org.hibernate.boot.MetadataBuilder`接口的`applyImplicitNamingStrategy`方法指定要使用的ImplicitNamingStrategy。 有关引导的更多详细信息可以[参考这里](/Book/bootstrap/README.md)。




