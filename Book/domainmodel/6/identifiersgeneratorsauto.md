 #### 2.6.7. Interpreting AUTO

    <div class="paragraph">

    How a persistence provider interprets the AUTO generation type is left up to the provider.

    </div>
    <div class="paragraph">

    The default behavior is to look at the java type of the identifier attribute.

    </div>
    <div class="paragraph">

    If the identifier type is UUID, Hibernate is going to use an [UUID identifier](#identifiers-generators-uuid).

    </div>
    <div class="paragraph">

    If the identifier type is numerical (e.g. `Long`, `Integer`), then Hibernate is going to use the `IdGeneratorStrategyInterpreter` to resolve the identifier generator strategy.
    The `IdGeneratorStrategyInterpreter` has two implementations:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`FallbackInterpreter`</dt>
    <dd>

    This is the default strategy since Hibernate 5.0. For older versions, this strategy is enabled through the [`hibernate.id.new_generator_mappings`](#configurations-mapping) configuration property .
    When using this strategy, `AUTO` always resolves to `SequenceStyleGenerator`.
    If the underlying database supports sequences, then a SEQUENCE generator is used. Otherwise, a TABLE generator is going to be used instead.

    </dd>
    <dt class="hdlist1">`LegacyFallbackInterpreter`</dt>
    <dd>

    This is a legacy mechanism that was used by Hibernate prior to version 5.0 or when the [`hibernate.id.new_generator_mappings`](#configurations-mapping) configuration property is false.
    The legacy strategy maps `AUTO` to the `native` generator strategy which uses the [Dialect#getNativeIdentifierGeneratorStrategy](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/dialect/Dialect.html#getNativeIdentifierGeneratorStrategy--) to resolve the actual identifier generator (e.g. `identity` or `sequence`).

    </dd>
    </dl>
    </div>
    </div>
    <div class="sect3">