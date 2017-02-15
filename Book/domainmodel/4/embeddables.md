 ### 2.4. Embeddable types

    <div class="paragraph">

    Historically Hibernate called these components.
    JPA calls them embeddables.
    Either way the concept is the same: a composition of values.
    For example we might have a Name class that is a composition of first-name and last-name, or an Address class that is a composition of street, city, postal code, etc.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="title">Usage of the word _embeddable_</div>
    <div class="paragraph">

    To avoid any confusion with the annotation that marks a given embeddable type, the annotation will be further referred as `@Embeddable`.

    </div>
    <div class="paragraph">

    Throughout this chapter and thereafter, for brevity sake, embeddable types may also be referred as _embeddable_.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="exampleblock">
    <div class="title">Example 78. Simple embeddable type example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Embeddable
    public class Name {

        private String firstName;

        private String middleName;

        private String lastName;

        ...
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Embeddable
    public class Address {

        private String line1;

        private String line2;

        @Embedded
        private ZipCode zipCode;

        ...

        @Embeddable
        public static class Zip {

            private String postalCode;

            private String plus4;

            ...
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    An embeddable type is another form of value type, and its lifecycle is bound to a parent entity type, therefore inheriting the attribute access from its parent (for details on attribute access, see [Access strategies](#access-embeddable-types)).

    </div>
    <div class="paragraph">

    Embeddable types can be made up of basic values as well as associations, with the caveat that, when used as collection elements, they cannot define collections themselves.

    </div>
    <div class="sect3">
