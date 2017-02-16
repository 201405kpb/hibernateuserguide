### 23.6. Bytecode Enhancement Properties

    <table class="tableblock frame-all grid-all spread">
    <colgroup>
    <col style="width: 20%;">
    <col style="width: 20%;">
    <col style="width: 60%;">
    </colgroup>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    Property
    </td>
    <td class="tableblock halign-left valign-top">

    Example
    </td>
    <td class="tableblock halign-left valign-top">

    Purpose
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.enhancer.enableDirtyTracking`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Enable dirty tracking feature in runtime bytecode enhancement.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.enhancer.enableLazyInitialization`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Enable lazy loading feature in runtime bytecode enhancement. This way, even basic types (e.g. `@Basic(fetch = FetchType.LAZY`)) can be fetched lazily.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.enhancer.enableAssociationManagement`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Enable association management feature in runtime bytecode enhancement which automatically synchronizes a bidirectional association when only one side is changed.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.bytecode.provider`
    </td>
    <td class="tableblock halign-left valign-top">

    `javassist` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    The [`BytecodeProvider`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/bytecode/spi/BytecodeProvider.html) built-in implementation flavor. Currently, only `javassist` is supported.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.bytecode.use_reflection_optimizer`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Should we use reflection optimization? The reflection optimizer implements the [`ReflectionOptimizer`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/bytecode/spi/ReflectionOptimizer.html) interface and improves entity instantiation and property getter/setter calls.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">