# 值类型

    A value type is a piece of data that does not define its own lifecycle.
    It is, in effect, owned by an entity, which defines its lifecycle.

    Looked at another way, all the state of an entity is made up entirely of value types.
    These state fields or JavaBean properties are termed _persistent attributes_.
    The persistent attributes of the `Contact` class are value types.


    Value types are further classified into three sub-categories:


    in mapping the `Contact` table, all attributes except for name would be basic types. Basic types are discussed in detail in [_Basic Types_](#basic)


    the name attribute is an example of an embeddable type, which is discussed in details in [_Embeddable Types_](#embeddables)


    Collection types


    although not featured in the aforementioned example, collection types are also a distinct category among value types. Collection types are further discussed in [_Collections_](#collections)
