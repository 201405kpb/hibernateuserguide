# 值类型

A value type is a piece of data that does not define its own lifecycle.It is, in effect, owned by an entity, which defines its lifecycle.
值类型是一种没有定义自己的生命周期的数据。它实际上由一个实体所拥有，实体定义了它的生命周期。

Looked at another way, all the state of an entity is made up entirely of value types.These state fields or JavaBean properties are termed persistent attributes.The persistent attributes of the `Contact` class are value types.
用另一种方式来看，实体的所有状态完全由值类型组成。这些状态字段或JavaBean属性称为持久属性。“Contact”类的持久属性是值类型。

Value types are further classified into three sub-categories:
值类型进一步分为三个子类：

Basic types in mapping the `Contact` table, all attributes except for name would be basic types. Basic types are discussed in detail in [Basic Types](#basic)
在`Contact`表中，除了name之外的所有属性都是基本类型。基本类型在[这里](#basic)进行详细讨论

Embeddable types the name attribute is an example of an embeddable type, which is discussed in details in [Embeddable Types](#embeddables)
Embeddable类型的name属性是一个可嵌入类型的例子。可嵌入类型在[这里](＃embeddables)进行详细讨论

Collection types although not featured in the aforementioned example, collection types are also a distinct category among value types. Collection types are further discussed in [_Collections_](#collections)
虽然集合类型在上述示例中没有对应，但集合类型也是值类型中的一种类别。 集合类型在[这里](＃collections)进一步讨论。


