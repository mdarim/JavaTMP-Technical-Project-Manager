Here’s a detailed explanation of what the @Embedded annotation in Java Persistence API (JPA) is — what it does, how it works together with @Embeddable (and related annotations), when to use it, and some caveats. I also give example code.

---

## ✅ What is @Embedded (and @Embeddable)

* In JPA, `@Embeddable` marks a class as **not a standalone entity**, but as a reusable component — a class whose instances can be embedded inside other entities. ([docs.jboss.org][1])
* Such an embeddable class **does not have its own identity / primary key** — it shares the identity of the enclosing entity. ([objectdb.com][2])
* The `@Embedded` annotation is used in an entity class to indicate that a field (or property) is such an embeddable object. JPA will map the fields of the embeddable object as columns in the **same table** as the owning entity. ([docs.jboss.org][3])
* In other words — instead of having a separate table for a logically nested value-object (like an address, name, period, etc.), JPA flattens the fields into the parent entity’s table. ([Baeldung on Kotlin][4])

This helps you model complex data (value-objects) in a more object-oriented, modular way — without cluttering your entity class with many primitive fields, and without proliferating separate entity/table classes when those “sub-objects” don’t need identity.

---

## 📄 Example — embedding value-object inside entity

Suppose you want an entity `Person` with a nested `Address`. Instead of putting all address-fields directly in `Person`, you can:

```java
@Embeddable
public class Address {
    private String street;
    private String city;
    private String country;
    private String zipcode;
    // + getters / setters
}

@Entity
public class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @Embedded           // <-- embed the value-object
    private Address address;

    // + getters / setters
}
```

Here:

* `Address` is annotated with `@Embeddable`, meaning it can be embedded. ([Learnitweb][5])
* In `Person`, the `address` field is annotated with `@Embedded`. JPA will map `street`, `city`, `country`, `zipcode` as columns in the **Person** table. ([Baeldung on Kotlin][4])

That way, you preserve a nice object-oriented design (an `Address` type), but without needing a separate `address` table.

---

## 🔧 Overriding Defaults and Customization

Because embedded classes may be reused in different entities — or you may want different column names — JPA provides ways to customize mapping:

* You can use `@AttributeOverride` (or `@AttributeOverrides`) on the embedded field to override column definitions inherited from the embeddable class. Example: rename `street` to `office_street`, etc. ([Oracle][6])
* If the embedded class itself contains embedded classes (nested embeddables), JPA supports that. ([docs.jboss.org][7])
* Note: an embeddable class must **not** define its own `@Id` or `@EmbeddedId` — because it doesn’t have independent identity. ([objectdb.com][2])

Also, if you only annotate the class with `@Embeddable`, sometimes JPA providers (e.g. Hibernate) infer the embedding even if you don’t explicitly annotate the field with `@Embedded`. ([Stack Overflow][8]) However, using `@Embedded` makes your intent clear and is considered good practice. ([Oracle Docs][9])

---

## 🧠 When / Why Use Embedded Types

Using `@Embedded` + `@Embeddable` is especially useful when:

* You have **value-objects** (e.g. Address, Name, Period, Money, etc.) that are logically part of an entity but don’t deserve their own identity or lifecycle.
* You want to **reuse** the same value-object across multiple entities (e.g. `BillingAddress` and `ShippingAddress` both use `Address`) without duplicating many fields.
* You want to **keep your entity classes cleaner**, avoiding dozens of primitive fields.
* You want to avoid extra tables and joins when the sub-object doesn’t need to be queried independently — reducing schema complexity and improving performance.

---

## ⚠️ Limitations & What It’s *Not* For

* Embedded objects don’t have an independent identity — you cannot `find()`, `delete()`, or manage them separately via an `EntityManager`. Their lifecycle is tied to the owning entity. ([java4coding.com][10])
* If what you thought was a value-object actually needs relationships (e.g. a collection, or a many-to-one to another entity), then embedding may not make sense — you should model it as a regular entity instead. Even though JPA allows some associations inside embeddables, it's more limited. ([objectdb.com][2])
* Overusing embedded classes — or deeply nested embeddables — can lead to awkward or hard-to-maintain code or mapping overrides, reducing clarity rather than improving it.
* If you need a composite primary key (i.e. multiple columns as primary key), you should use `@EmbeddedId` (on an embeddable class) — another use of embeddables. ([Oracle][6])

---

## 📚 Summary

* `@Embeddable` marks a class as a reusable value-object type (no identity, no separate table) to be embedded in entities.
* `@Embedded` (in the entity) tells JPA to embed that value-object into the entity’s table as columns.
* Embedded types help keep the object model clean and avoid unnecessary tables for value-objects.
* Use overrides (`@AttributeOverride`, etc.) if you need custom column names or reuse the embeddable in different contexts.
* Embedded types are **not** entities — they don’t have their own identity or lifecycle.

[1]: https://docs.jboss.org/hibernate/jpa/2.2/api/javax/persistence/Embeddable.html?utm_source=chatgpt.com "Embeddable (javax.persistence-api 2.2 API)"
[2]: https://www.objectdb.com/api/java/jpa/Embeddable?utm_source=chatgpt.com "jakarta.persistence.Embeddable - JPA Annotation - JPA API Reference"
[3]: https://docs.jboss.org/hibernate/jpa/2.1/api/javax/persistence/Embedded.html?utm_source=chatgpt.com "Embedded (hibernate-jpa-2.1-api 1.0.0.Final API)"
[4]: https://www.baeldung.com/jpa-embedded-embeddable?utm_source=chatgpt.com "Jpa @Embedded and @Embeddable | Baeldung"
[5]: https://learnitweb.com/hibernate-tutorial/jpa-embedded-and-embeddable/?utm_source=chatgpt.com "JPA @Embedded And @Embeddable - Learnitweb"
[6]: https://www.oracle.com/middleware/technologies/toplink-jpa-annotations.html?utm_source=chatgpt.com "TopLink JPA Annotation Reference"
[7]: https://docs.jboss.org/hibernate/jpa/2.1/api/javax/persistence/Embeddable.html?utm_source=chatgpt.com "Embeddable (hibernate-jpa-2.1-api 1.0.0.Final API)"
[8]: https://stackoverflow.com/questions/31157700/is-the-jpa-embedded-annotation-mandatory?utm_source=chatgpt.com "java - Is the JPA @Embedded annotation mandatory? - Stack Overflow"
[9]: https://docs.oracle.com/javaee/7/api/javax/persistence/Embedded.html?utm_source=chatgpt.com "Embedded (Java(TM) EE 7 Specification APIs)"
[10]: https://www.java4coding.com/contents/jpa/embeddable-and-embedded-annotation?utm_source=chatgpt.com "JPA @Embeddable and @Embedded Annotation - java4coding"

Cool — here are **3 real-world example** sets of entity / embeddable classes using `@Embedded` / `@Embeddable` to show how you might model domain-type data in a clean, modular way with JPA.

---

## ✅ Example 1 — Person with Name & Address (reusable value-objects)

```java
// Name.java  
@Embeddable
public class Name {
    private String firstName;
    private String middleName;
    private String lastName;

    // constructors, getters / setters
}

// Address.java  
@Embeddable
public class Address {
    private String street;
    private String city;
    private String state;
    private String country;
    private String zipCode;

    // constructors, getters / setters
}

// Person.java  
@Entity
public class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Embedded
    private Name name;

    private String email;

    @Embedded
    private Address address;

    // constructors, getters / setters
}
```

**What this does:**

* `Name` and `Address` are annotated with `@Embeddable` — indicating they’re *value-objects*, not full-blown entities. ([logicbig.com][1])
* `Person` uses `@Embedded` on `name` and `address`, so the fields from `Name` and `Address` get mapped into the same database table as `Person`. ([Learnitweb][2])
* This keeps the domain model modular and clean (you reuse `Name`, `Address` across entities), without introducing extra tables. ([CalliCoder][3])

---

## 🛒 Example 2 — Order with a Money (or Price) Value-Object, plus Billing & Shipping Addresses

Often in an e-commerce or order-processing domain you want to treat things like money, price, or structured address as value-objects rather than entities. Embeddables are great for this.

```java
// Money.java  
@Embeddable
public class Money {
    private BigDecimal amount;
    private String currency;

    // constructors, getters / setters, maybe utility methods
}

// Address.java  
@Embeddable
public class Address {
    private String street;
    private String city;
    private String state;
    private String country;
    private String postalCode;

    // constructors, getters / setters
}

// Order.java  
@Entity
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Embedded
    private Money totalAmount;

    @Embedded
    @AttributeOverrides({
        @AttributeOverride(name = "street", column = @Column(name = "billing_street")),
        @AttributeOverride(name = "city",   column = @Column(name = "billing_city")),
        @AttributeOverride(name = "state",  column = @Column(name = "billing_state")),
        @AttributeOverride(name = "country",column = @Column(name = "billing_country")),
        @AttributeOverride(name = "postalCode", column = @Column(name = "billing_postal_code"))
    })
    private Address billingAddress;

    @Embedded
    @AttributeOverrides({
        @AttributeOverride(name = "street", column = @Column(name = "shipping_street")),
        @AttributeOverride(name = "city",   column = @Column(name = "shipping_city")),
        @AttributeOverride(name = "state",  column = @Column(name = "shipping_state")),
        @AttributeOverride(name = "country",column = @Column(name = "shipping_country")),
        @AttributeOverride(name = "postalCode", column = @Column(name = "shipping_postal_code"))
    })
    private Address shippingAddress;

    // other fields like orderDate, items, etc.
}
```

**Notes:**

* The `Money` class is a simple embeddable representing structured data (amount + currency), not requiring its own table or identity. This models domain-level “value-object” semantics.
* For two logical “Address” fields (billing vs shipping), we embed `Address` twice and use `@AttributeOverrides` to avoid column name clashes in the table. This is a common pattern. ([Java Code Geeks][4])
* This keeps the data model clean, avoids extra join/tables for simple value-objects.

---

## 🧰 Example 3 — Nested / Composite Embeddables

You can even embed an embeddable class inside another embeddable — useful for modeling nested value-objects.

```java
// ZipCode.java  
@Embeddable
public class ZipCode {
    private String main;
    private String plusFour;

    // constructors, getters / setters
}

// Address.java  
@Embeddable
public class Address {
    private String street;
    private String city;

    @Embedded
    private ZipCode zipCode;

    // constructors, getters / setters
}

// Customer.java  
@Entity
public class Customer {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @Embedded
    private Address address;

    // other fields...
}
```

**What to notice:**

* `ZipCode` is a small embeddable representing a sub-component (e.g. postal code with main & extension). Then `Address` embeds `ZipCode`, and `Customer` embeds `Address`. This gives nested value-object composition. ([logicbig.com][5])
* All columns (street, city, zipCode.main, zipCode.plusFour) end up in the `Customer` table. No extra tables — just a flattened structure at DB level, but object-oriented structure at code level.

---

## 🎯 When to Use These Patterns — and When Not

Use `@Embeddable` / `@Embedded` when:

* You have **value-objects** (address, money, name, period, coordinates, etc.) that logically belong to an entity and don’t need their own identity.
* You want to **reuse** such value-objects across multiple entities (e.g. Address might be reused in `User`, `Company`, `Order`, etc.).
* You want to **avoid extra tables/joins** when the embedded data doesn’t need separate lifecycle, queries or relationships.

Avoid embedding when:

* The sub-object needs an independent identity, relationships or lifecycle. Then it should be a real `@Entity`.
* You expect to query or manage sub-objects independently (e.g. find all addresses, or join on address). For those cases, embedding is too limiting.

[1]: https://www.logicbig.com/tutorials/java-ee-tutorial/jpa/embeddable-classes.html?utm_source=chatgpt.com "JPA + Hibernate - Embeddable Classes"
[2]: https://learnitweb.com/hibernate-tutorial/jpa-embedded-and-embeddable/?utm_source=chatgpt.com "JPA @Embedded And @Embeddable - Learnitweb"
[3]: https://www.callicoder.com/hibernate-spring-boot-jpa-embeddable-demo/?utm_source=chatgpt.com "JPA / Hibernate @Embeddable and @Embedded Example with Spring Boot | CalliCoder"
[4]: https://www.javacodegeeks.com/2020/01/jpa-embeddable-and-embedded.html?utm_source=chatgpt.com "JPA @Embeddable and @Embedded - Java Code Geeks"
[5]: https://www.logicbig.com/tutorials/java-ee-tutorial/jpa/chain-of-embeddable.html?utm_source=chatgpt.com "JPA + Hibernate - Embeddable class within another embeddable class Example"
