### 14.3. Mixing Events and Interceptors

    <div class="paragraph">

    When you want to customize the entity state transition behavior, you have to options:

    </div>
    <div class="olist arabic">

1.  you provide a custom `Interceptor`, which is taken into consideration by the default Hibernate event listeners.
    For example, the `Interceptor#onSave()` method is invoked by Hibernate `AbstractSaveEventListener`.
    Or, the `Interceptor#onLoad()` is called by the `DefaultPreLoadEventListener`.
2.  you can replace any given default event listener with your own implementation.
    When doing this, you should probably extend the default listeners because otherwise you&#8217;d have to take care of all the low-level entity state transition logic.
    For example, if you replace the `DefaultPreLoadEventListener` with your own implementation, then, only if you call the `Interceptor#onLoad()` method explicitly, you can mix the custom load event listener with a custom Hibernate interceptor.
    </div>
    </div>
    <div class="sect2">