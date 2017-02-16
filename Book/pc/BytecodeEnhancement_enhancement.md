 #### 5.2.2. Performing enhancement

    <div class="sect4">

    ##### Run-time enhancement

    <div class="paragraph">

    Currently, run-time enhancement of the domain model is only supported in managed JPA environments following the JPA-defined SPI for performing class transformations.
    Even then, this support is disabled by default.
    To enable run-time enhancement, specify `hibernate.ejb.use_class_enhancer`=`true` as a persistent unit property.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Also, at the moment, only annotated classes are supported for run-time enhancement.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect4">

    ##### Gradle plugin

    <div class="paragraph">

    Hibernate provides a Gradle plugin that is capable of providing build-time enhancement of the domain model as they are compiled as part of a Gradle build.
    To use the plugin a project would first need to apply it:

    </div>
    <div class="exampleblock">
    <div class="title">Example 226. Apply the Gradle plugin</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`ext {
        hibernateVersion = 'hibernate-version-you-want'
    }

    buildscript {
        dependencies {
            classpath "org.hibernate:hibernate-gradle-plugin:$hibernateVersion"
        }
    }

    hibernate {
        enhance {
            // any configuration goes here
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The configuration that is available is exposed through a registered Gradle DSL extension:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">enableLazyInitialization</dt>
    <dd>

    Whether enhancement for lazy attribute loading should be done.

    </dd>
    <dt class="hdlist1">enableDirtyTracking</dt>
    <dd>

    Whether enhancement for self-dirty tracking should be done.

    </dd>
    <dt class="hdlist1">enableAssociationManagement</dt>
    <dd>

    Whether enhancement for bi-directional association management should be done.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    The default value for all 3 configuration settings is `true`

    </div>
    <div class="paragraph">

    The `enhance { }` block is required in order for enhancement to occur.
    Enhancement is disabled by default in preparation for additions capabilities (hbm2ddl, etc) in the plugin.

    </div>
    </div>
    <div class="sect4">

    ##### Maven plugin

    <div class="paragraph">

    Hibernate provides a Maven plugin capable of providing build-time enhancement of the domain model as they are compiled as part of a Maven build.
    See the section on the [Gradle plugin](#BytecodeEnhancement-enhancement-gradle) for details on the configuration settings. Again, the default for those 3 is `true`.

    </div>
    <div class="paragraph">

    The Maven plugin supports one additional configuration settings: failOnError, which controls what happens in case of error.
    Default behavior is to fail the build, but it can be set so that only a warning is issued.

    </div>
    <div class="exampleblock">
    <div class="title">Example 227. Apply the Maven plugin</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;build&gt;
        &lt;plugins&gt;
            [...]
            &lt;plugin&gt;
                &lt;groupId&gt;org.hibernate.orm.tooling&lt;/groupId&gt;
                &lt;artifactId&gt;hibernate-enhance-maven-plugin&lt;/artifactId&gt;
                &lt;version&gt;$currentHibernateVersion&lt;/version&gt;
                &lt;executions&gt;
                    &lt;execution&gt;
                        &lt;configuration&gt;
                            &lt;failOnError&gt;true&lt;/failOnError&gt;
                            &lt;enableLazyInitialization&gt;true&lt;/enableLazyInitialization&gt;
                            &lt;enableDirtyTracking&gt;true&lt;/enableDirtyTracking&gt;
                            &lt;enableAssociationManagement&gt;true&lt;/enableAssociationManagement&gt;
                        &lt;/configuration&gt;
                        &lt;goals&gt;
                            &lt;goal&gt;enhance&lt;/goal&gt;
                        &lt;/goals&gt;
                    &lt;/execution&gt;
                &lt;/executions&gt;
            &lt;/plugin&gt;
            [...]
        &lt;/plugins&gt;
    &lt;/build&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">