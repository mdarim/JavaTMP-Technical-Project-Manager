Good question. Short answer: **by default**, with Jakarta Persistence API (JPA) / Hibernate / common implementations: the field annotated with @Version is expected to be a **numeric (or timestamp)** field — *not* a random or UUID — in order to support the built-in optimistic-locking mechanism. ([datanucleus.org][1])

Here’s more detail:

---

## ✅ What JPA expects by default for `@Version`

* The spec says the version field/property must be a numeric type: `int`, `long`, `short`, or their wrapper equivalents (e.g. `Integer`, `Long`), or a timestamp/date type. ([datanucleus.org][2])
* On each update, the provider (Hibernate / JPA) will automatically increment the version (or update the timestamp). ([learn-it-university.com][3])
* During commit, JPA uses this version value to check whether the row was modified by another transaction; if so, it throws `OptimisticLockException`. ([learn-it-university.com][3])

Because of this incremental check, numeric or timestamp fields are ideal — JPA knows how to compare and increment them.

---

## ⚠️ Why a random number or UUID isn’t standard for `@Version`

* Since JPA expects to **increment** the version automatically on each update, a random value (or UUID) doesn’t follow the “increment-by-one (or timestamp update)” semantics.
* The standard JPA mapping guides (e.g., for DataNucleus) specifically list numeric and date/time types as valid version types. ([datanucleus.org][1])
* Using a UUID (or random) for versioning breaks the standard mechanism: JPA wouldn’t know how to “increment” it or detect “old vs new version” in the usual way.

---

## 🔀 What about using UUID / random for *IDs* (primary key) instead of version

That **is** common. For primary key (not version), many use UUIDs for global uniqueness, especially in distributed systems. ([jivimberg.io][4])
But that’s a different use — UUID as identifier vs version for optimistic locking.

---

## 🧪 If you really want random/UUID-based versioning: possible, but custom

Technically you could implement your own versioning logic — for example:

```java
@Entity
public class Document {
   @Id
   private Long id;

   @Version
   private UUID version = UUID.randomUUID();

   @PreUpdate
   public void updateVersion() {
      this.version = UUID.randomUUID();
   }
}
```

But:

* This is *non-standard*. The JPA spec doesn’t define how to handle UUID version fields.
* You lose the standard “increment + compare” semantics — you need to ensure the uniqueness/consistency yourself.
* Portability and support across JPA providers becomes uncertain.

In fact, some articles recommending this approach come from external sources (not core spec). ([blogs.businesscompassllc.com][5])

---

## ✅ Conclusion — What to use

Use a **numeric (int/long) or timestamp-based** field with `@Version`. That’s the standard, supported, and reliable way for optimistic locking.
If you need globally unique IDs, use a separate `@Id` with UUID (or serial, sequence, etc.), but don’t mix UUID as a version.

[1]: https://www.datanucleus.org/products/accessplatform_5_0/jpa/mapping.pdf?utm_source=chatgpt.com "JPA Mapping Guide (v5.0)"
[2]: https://www.datanucleus.org/products/accessplatform_6_0/jpa/mapping.pdf?utm_source=chatgpt.com "JPA Mapping Guide (v6.0)"
[3]: https://learn-it-university.com/understanding-the-version-annotation-in-java-jpa/?utm_source=chatgpt.com "Mastering Concurrency in Java: Understanding the @Version Annotation in JPA - Learn IT University"
[4]: https://jivimberg.io/blog/2018/11/05/using-uuid-on-spring-data-jpa-entities?utm_source=chatgpt.com "Using UUID on Spring Data JPA Entities - Coding Forest"
[5]: https://blogs.businesscompassllc.com/2025/11/boost-performance-with-optimistic.html?utm_source=chatgpt.com "Boost Performance with Optimistic Locking | Java JPA & SQL Examples"

Cool — here are two sample code patterns:

---

## ✅ Conventional (Standard): Numeric or Timestamp Version + maybe UUID as Primary Key

This is the standard, JPA-managed “optimistic locking + UUID id” pattern.

```java
import javax.persistence.*;
import java.util.UUID;

@Entity
public class MyEntity {
    @Id
    @Column(name = "id", updatable = false, nullable = false)
    private UUID id = UUID.randomUUID();

    private String name;

    @Version
    private long version;     // numeric version for optimistic locking

    // constructors, getters, setters…

    protected MyEntity() { }  // for JPA

    public MyEntity(String name) {
        this.name = name;
    }
}
```

**What happens:**

* The `id` is a UUID — unique, suitable for distributed systems, etc. This is independent of versioning.
* The `version` field (long) is managed by JPA/Hibernate: on each update, its value increments automatically. ([Baeldung on Kotlin][1])
* When you commit an update, JPA includes `WHERE ... version = ?` in the SQL. If another transaction already updated the row (i.e. version changed), it fails with an `OptimisticLockException`. ([DZone][2])
* You do *not* manually touch `version`. That’s the point of `@Version`. ([Baeldung on Kotlin][1])

This pattern keeps your primary key and version semantics cleanly separated, works reliably, and is the most common recommended way.

---

## 🔧 Custom / Non-Standard: UUID (or random) for Version

If you really prefer using a UUID (or random token) as “version”, you can do that — but you must manage updates yourself. That means giving up the default automatic increment behavior of JPA’s `@Version`.

```java
import javax.persistence.*;
import java.util.UUID;

@Entity
public class CustomVersionEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String data;

    @Version
    @Column(name = "version_uuid", nullable = false)
    private UUID version = UUID.randomUUID();

    @PreUpdate
    private void updateVersion() {
        this.version = UUID.randomUUID();
    }

    // constructors, getters, setters…

    protected CustomVersionEntity() { }

    public CustomVersionEntity(String data) {
        this.data = data;
    }
}
```

**What’s going on:**

* `id` is a simple generated primary key (here `Long`).
* `version` is a `UUID`. We initially assign a random UUID, and before each update we re-assign a new random UUID (via `@PreUpdate`).
* Since `version` changes on each update, conceptually it works like a “logical timestamp” / “token” — two concurrent transactions that started with the same version UUID will conflict, because after one commits, the UUID changes.
* But **this is not standard JPA behavior**: JPA providers expect version fields to be numeric (or timestamp/date) types, and will typically use numeric increment or timestamp update. ([datanucleus.org][3])
* Therefore this approach may lead to undefined or unsupported behavior: portability across JPA implementations is not guaranteed, and you may run into subtle bugs (e.g. JPA swallowing updates, not detecting conflicts correctly, or not honoring version semantics).

---

## 🧑‍💼 Recommendation: Stick with Numeric or Timestamp for @Version

* Using numeric (or timestamp) `@Version` + separate `@Id` (possibly UUID) is the standard, well-supported approach. It works reliably across JPA providers.
* If you’re tempted to use UUID/random version: only do this if you have **very special requirements** (e.g. token-based versioning, distributed versioning across systems, etc.), and you are ready to accept potential downsides in portability and maintainability.

[1]: https://www.baeldung.com/jpa-optimistic-locking?utm_source=chatgpt.com "Optimistic Locking in JPA | Baeldung"
[2]: https://dzone.com/articles/jpa-20-concurrency-and-locking?utm_source=chatgpt.com "JPA 2.0 Concurrency and Locking"
[3]: https://www.datanucleus.org/products/accessplatform_6_0/jpa/mapping.pdf?utm_source=chatgpt.com "JPA Mapping Guide (v6.0)"

Cool — here is how you can use a **timestamp (date/time)** column for versioning in JPA (with Hibernate or a JPA provider compatible with timestamp-versioning).

---

## ✅ Using Timestamp for `@Version`

You can annotate a `Date`, `Calendar`, or — in newer Hibernate versions — a `java.time` date/time class (e.g. `LocalDateTime`) with `@Version`. The ORM will then use that timestamp for optimistic locking instead of a numeric counter. ([docs.jboss.org][1])

Here’s an example:

```java
import jakarta.persistence.*;
import java.time.LocalDateTime;

@Entity
public class MyEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @Version
    private LocalDateTime version;  
    // Alternative: private Date version;

    // constructors, getters, setters
}
```

**What happens:**

* On each update, Hibernate updates the `version` field with the current timestamp. ([docs.jboss.org][2])
* When Hibernate issues the `UPDATE`, it includes `WHERE version = ?` similar to integer versioning. That way, if another transaction already updated the row (and changed the timestamp), your update will fail with an optimistic-locking exception. ([Thorben Janssen][3])

You may also have the timestamp generated by the database (rather than by the JVM), using appropriate annotations — e.g. for a database-generated current timestamp. ([docs.jboss.org][2])

---

## 🧑‍💻 Full Example with `LocalDateTime` Version

Assuming you use Hibernate 5+ or 6, here is a full entity example:

```java
import jakarta.persistence.*;
import java.time.LocalDateTime;

@Entity
public class Document {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String content;

    @Version
    private LocalDateTime version;

    // default constructor for JPA
    protected Document() { }

    public Document(String content) {
        this.content = content;
    }

    // getters and setters...
}
```

If you prefer to have the DB generate the timestamp (for example via `CURRENT_TIMESTAMP`), you could configure the column accordingly (via DDL or using generator annotations depending on DB/provider support). ([docs.jboss.org][2])

---

## ⚠️ Considerations & Trade-offs

* Timestamp-based versioning is **supported**, but many consider it **less robust** than numeric versioning. ([docs.jboss.org][2])
* Because the version is a timestamp, comparisons still use equality (`=`), not “less than / greater than”. That means two updates with very close timestamps may still conflict (or not detect conflict if timestamp precision is coarse). ([Hibernate][4])
* If you rely on human-readable “last updated” semantics, timestamp versioning doubles as “last modified date,” which can be convenient. But beware — some JPA providers may generate the first timestamp slightly before the database-generated creation timestamp (on initial insert), so `createTime == updateTime` may not hold reliably. ([OCTO Talks !][5])

[1]: https://docs.jboss.org/hibernate/orm/6.3/userguide/html_single/Hibernate_User_Guide.html?utm_source=chatgpt.com "Hibernate ORM User Guide"
[2]: https://docs.jboss.org/hibernate/orm/6.2/userguide/html_single/Hibernate_User_Guide.html?utm_source=chatgpt.com "Hibernate ORM User Guide"
[3]: https://thorben-janssen.com/hibernate-tips-use-timestamp-versioning-optimistic-locking/?utm_source=chatgpt.com "Hibernate Tips: How to use a timestamp for versioning and optimistic locking"
[4]: https://discourse.hibernate.org/t/is-it-possible-to-customise-the-version-comparison-when-using-timestamp/7543?utm_source=chatgpt.com "Is it possible to customise the @Version comparison when using Timestamp? - Hibernate ORM - Hibernate"
[5]: https://blog.octo.com/audit-with-jpa-creation-and-update-date?utm_source=chatgpt.com "Audit with JPA: creation and update date - OCTO Talks !"
