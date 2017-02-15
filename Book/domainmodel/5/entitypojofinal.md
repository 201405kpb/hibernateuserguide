 #### 2.5.2. Prefer non-final classes

    <div class="paragraph">

    A central feature of Hibernate is the ability to load lazily certain entity instance variables (attributes) via runtime proxies.
    This feature depends upon the entity class being non-final or else implementing an interface that declares all the attribute getters/setters.
    You can still persist final classes that do not implement such an interface with Hibernate,
    but you will not be able to use proxies for fetching lazy associations, therefore limiting your options for performance tuning.
    For the very same reason, you should also avoid declaring persistent attribute getters and setters as final.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Starting in 5.0 Hibernate offers a more robust version of bytecode enhancement as another means for handling lazy loading.
    Hibernate had some bytecode re-writing capabilities prior to 5.0 but they were very rudimentary.
    See the [BytecodeEnhancement](#BytecodeEnhancement) for additional information on fetching and on bytecode enhancement.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">