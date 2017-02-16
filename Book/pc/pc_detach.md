### 5.10. Working with detached data

    <div class="paragraph">

    Detachment is the process of working with data outside the scope of any persistence context.
    Data becomes detached in a number of ways.
    Once the persistence context is closed, all data that was associated with it becomes detached.
    Clearing the persistence context has the same effect.
    Evicting a particular entity from the persistence context makes it detached.
    And finally, serialization will make the deserialized form be detached (the original instance is still managed).

    </div>
    <div class="paragraph">

    Detached data can still be manipulated, however the persistence context will no longer automatically know about these modification and the application will need to intervene to make the changes persistent again.

    </div>
    <div class="sect3">