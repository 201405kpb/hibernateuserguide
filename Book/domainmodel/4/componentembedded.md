 #### 2.4.1. Component / Embedded

    <div class="paragraph">

    Most often, embeddable types are used to group multiple basic type mappings and reuse them across several entities.

    </div>
    <div class="exampleblock">
    <div class="title">Example 79. Simple Embeddedable</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Person {

        @Id
        private Integer id;

        @Embedded
        private Name name;

        ...
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

    JPA defines two terms for working with an embeddable type: `@Embeddable` and `@Embedded`.
    `@Embeddable` is used to describe the mapping type itself (e.g. `Name`).
    `@Embedded` is for referencing a given embeddable type (e.g. `person.name`).

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    So, the embeddable type is represented by the `Name` class and the parent makes use of it through the `person.name` object composition.

    </div>
    <div class="exampleblock">
    <div class="title">Example 80. Person table</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`create table Person (
        id integer not null,
        firstName VARCHAR,
        middleName VARCHAR,
        lastName VARCHAR,
        ...
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The composed values are mapped to the same table as the parent table.
    Composition is part of good OO data modeling (idiomatic Java).
    In fact, that table could also be mapped by the following entity type instead.

    </div>
    <div class="exampleblock">
    <div class="title">Example 81. Alternative to embeddable type composition</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class Person {

        @Id
        private Integer id;

        private String firstName;

        private String middleName;

        private String lastName;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The composition form is certainly more Object-oriented, and that becomes more evident as we work with multiple embeddable types.

    </div>
    </div>
    <div class="sect3">