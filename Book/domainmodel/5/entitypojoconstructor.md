
    #### 2.5.3. Implement a no-argument constructor

    <div class="paragraph">

    The entity class should have a no-argument constructor. Both Hibernate and JPA require this.

    </div>
    <div class="paragraph">

    JPA requires that this constructor be defined as public or protected.
    Hibernate, for the most part, does not care about the constructor visibility, as long as the system SecurityManager allows overriding the visibility setting.
    That said, the constructor should be defined with at least package visibility if you wish to leverage runtime proxy generation.

    </div>
    </div>
    <div class="sect3">