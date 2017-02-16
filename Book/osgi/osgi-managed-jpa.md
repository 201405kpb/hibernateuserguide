### 20.5. Container-Managed JPA

<div class="paragraph">

The Enterprise OSGi specification includes container-managed JPA.
The container is responsible for discovering persistence units in bundles and automatically creating the `EntityManagerFactory` (one `EntityManagerFactory` per `PersistenceUnit`).
It uses the JPA provider (hibernate-osgi) that has registered itself with the OSGi `PersistenceProvider` service.

</div>
</div>
<div class="sect2">