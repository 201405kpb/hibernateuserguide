    ### 5.2. Bytecode Enhancement

    <div class="paragraph">

    Hibernate "grew up" not supporting bytecode enhancement at all.
    At that time, Hibernate only supported proxy-based for lazy loading and always used diff-based dirty calculation.
    Hibernate 3.x saw the first attempts at bytecode enhancement support in Hibernate.
    We consider those initial attempts (up until 5.0) completely as an incubation.
    The support for bytecode enhancement in 5.0 onward is what we are discussing here.

    </div>
    <div class="sect3">