 #### 24.1.13. `@Converter`

    <div class="paragraph">

    The [`@Converter`](http://docs.oracle.com/javaee/7/api/javax/persistence/Converter.html) annotation is used to specify that the current annotate [`AttributeConverter`](http://docs.oracle.com/javaee/7/api/javax/persistence/AttributeConverter.html) implementation can be used as a JPA basic attribute converter.

    </div>
    <div class="paragraph">

    If the [`autoApply`](http://docs.oracle.com/javaee/7/api/javax/persistence/Converter.html#autoApply--) attribute is set to `true`, then the JPA provider will automatically convert all basic attributes with the same Java type as defined by the current converter.

    </div>
    <div class="paragraph">

    See the [`AttributeConverter`](#basic-enums-attribute-converter) section for more info.

    </div>
    </div>
    <div class="sect3">