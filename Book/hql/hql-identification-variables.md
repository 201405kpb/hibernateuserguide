 ### 15.12. Identification variables

    <div class="paragraph">

    Identification variables are often referred to as aliases.
    References to object model classes in the `FROM` clause can be associated with an identification variable that can then be used to refer to that type throughout the rest of the query.

    </div>
    <div class="paragraph">

    In most cases declaring an identification variable is optional, though it is usually good practice to declare them.

    </div>
    <div class="paragraph">

    An identification variable must follow the rules for Java identifier validity.

    </div>
    <div class="paragraph">

    According to JPQL, identification variables must be treated as case-insensitive.
    Good practice says you should use the same case throughout a query to refer to a given identification variable.
    In other words, JPQL says they _can be_ case-insensitive and so Hibernate must be able to treat them as such, but this does not make it good practice.

    </div>
    </div>
    <div class="sect2">