#### 24.1.12. `@Convert`

    <div class="paragraph">

    The [`@Convert`](http://docs.oracle.com/javaee/7/api/javax/persistence/Convert.html) annotation is used to specify the [`AttributeConverter`](http://docs.oracle.com/javaee/7/api/javax/persistence/AttributeConverter.html) implementation used to convert the current annotated basic attribute.

    </div>
    <div class="paragraph">

    If the `AttributeConverter` uses [`autoApply`](http://docs.oracle.com/javaee/7/api/javax/persistence/Converter.html#autoApply--), then all entity attributes with the same target type are going to be converted automatically.

    </div>
    <div class="paragraph">

    See the [`AttributeConverter`](#basic-enums-attribute-converter) section for more info.

    </div>
    </div>
    <div class="sect3">
