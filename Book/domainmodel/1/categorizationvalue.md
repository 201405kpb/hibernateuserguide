#值类型

 <div class="paragraph">

    A value type is a piece of data that does not define its own lifecycle.
    It is, in effect, owned by an entity, which defines its lifecycle.

    </div>
    <div class="paragraph">

    Looked at another way, all the state of an entity is made up entirely of value types.
    These state fields or JavaBean properties are termed _persistent attributes_.
    The persistent attributes of the `Contact` class are value types.

    </div>
    <div class="paragraph">

    Value types are further classified into three sub-categories:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">Basic types</dt>
    <dd>

    in mapping the `Contact` table, all attributes except for name would be basic types. Basic types are discussed in detail in [_Basic Types_](#basic)

    </dd>
    <dt class="hdlist1">Embeddable types</dt>
    <dd>

    the name attribute is an example of an embeddable type, which is discussed in details in [_Embeddable Types_](#embeddables)

    </dd>
    <dt class="hdlist1">Collection types</dt>
    <dd>

    although not featured in the aforementioned example, collection types are also a distinct category among value types. Collection types are further discussed in [_Collections_](#collections)

    </dd>
    </dl>
    </div>
    </div>
    <div class="sect3">
