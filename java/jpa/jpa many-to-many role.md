Cool — here’s a more complete example of how you might implement a **many‑to‑many Roles ↔ Permissions + Users ↔ Roles + row‑level filtering / permission‑aware access** in a Spring Boot + Spring Data JPA setup. The idea is that:

* Users have many Roles
* Roles have many Permissions
* On a domain entity (e.g. `Document`, `Resource`, `Project`, etc.), you define which permission(s) are required to view or act on it
* At query time / service time, you filter data based on the current user’s effective permissions

Below is a **sample design + code sketch + flow**.

---

## 📚 Data model: User ↔ Role ↔ Permission + Domain‑Entity ↔ required Permission(s)

```text
User ↔ ManyToMany ↔ Role  
Role ↔ ManyToMany ↔ Permission  
DomainEntity (e.g. Document) ↔ ManyToMany ↔ Permission  // or some link that indicates which permissions apply
```

### Example JPA Entities

```java
// Permission.java
@Entity
public class Permission {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true, nullable = false)
    private String name;   // e.g. "DOC_READ", "DOC_EDIT", "PROJECT_VIEW", etc.

    // equals / hashCode by id or name
}

// Role.java
@Entity
public class Role {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique=true, nullable=false)
    private String name;   // e.g. "ADMIN", "EDITOR", "USER"

    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
      name = "roles_permissions",
      joinColumns = @JoinColumn(name="role_id"),
      inverseJoinColumns = @JoinColumn(name="permission_id")
    )
    private Set<Permission> permissions = new HashSet<>();
}

// User.java
@Entity
public class User {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;
    private String password;

    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
      name = "users_roles",
      joinColumns = @JoinColumn(name="user_id"),
      inverseJoinColumns = @JoinColumn(name="role_id")
    )
    private Set<Role> roles = new HashSet<>();

    @Transient
    public Set<String> getPermissionNames() {
        return roles.stream()
                    .flatMap(r -> r.getPermissions().stream())
                    .map(Permission::getName)
                    .collect(Collectors.toSet());
    }
}
```

Then a sample domain entity:

```java
// Document.java
@Entity
public class Document {
  @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  private String title;
  private String content;

  @ManyToMany(fetch = FetchType.LAZY)
  @JoinTable(
    name = "document_permissions",
    joinColumns = @JoinColumn(name = "document_id"),
    inverseJoinColumns = @JoinColumn(name = "permission_id")
  )
  private Set<Permission> allowedPermissions = new HashSet<>();

  // e.g. other metadata like owner, creation date, etc.
}
```

This model means: each document says “these permissions allow access to this document.” A user with any of those permissions (via their roles) can read or act on the document.

---

## 🔐 Security + Data‑access logic

* Use Spring Security (or similar) to store authentication/authorization (users, roles, permissions). Many tutorials rely on roles, but you can treat permissions as “authorities.” ([Medium][1])
* On login / token creation (e.g. with JWT) include user's roles/permissions. ([كود جافا][2])
* In service layer, when querying domain‑data like `Document`, filter based on user’s permissions.

---

## 🧮 Querying / Filtering with JPA + Specifications + Permission Check

You can use JpaSpecificationExecutor along with custom specifications to filter by permission:

```java
public class DocumentSpecifications {
    public static Specification<Document> accessibleBy(User user) {
        return (root, cq, cb) -> {
            Set<String> perms = user.getPermissionNames();
            if (perms.contains("ADMIN")) {
                return cb.conjunction();
            }
            // join document ↔ permissions
            Join<Document, Permission> permsJoin = root.join("allowedPermissions");
            return permsJoin.get("name").in(perms);
        };
    }
}

// Repository
public interface DocumentRepository extends JpaRepository<Document, Long>,
                                             JpaSpecificationExecutor<Document> {
}
```

Then in service:

```java
User current = ... // get from Spring Security context
List<Document> docs = docRepo.findAll(DocumentSpecifications.accessibleBy(current));
```

This way DB returns only documents for which the user has at least one of the required permissions (via their roles). This filtering happens at query level.

This is a generalization of the pattern described by the “filter with join‑tables via Specification” approach. StackOverflow has a related example of how to filter ManyToMany relationships using Specifications. ([Stack Overflow][3])

---

## ✅ Why this design works — and what trade‑offs to consider

**Pros**:

* Very flexible: you can add/remove permissions or roles in DB without changing code logic.
* Fine‑grained control: not just “ROLE_ADMIN vs ROLE_USER” but actual permissions like “DOC_EDIT”, “DOC_VIEW”.
* DB‑level filtering: only authorized rows are fetched, saving memory & avoiding post‑fetch filtering inefficiencies.

**Cons / Things to watch out for**:

* Complexity: mapping and managing many‑to‑many relationships (user ↔ role ↔ permission, plus domain ↔ permission) is more complex than simple role-based checks.
* Overhead: joins + permission checks in queries — for large data sets, might impact performance. Use pagination, indexes, etc.
* Business‑logic clarity: you must carefully define which permissions apply to which resources; requirement complexity can grow.

---

## 🧪 When and how to combine with Method‑level Security (Spring Security)

Even with data‑level filtering, you should also enforce method / endpoint access using Spring Security annotations, e.g.:

```java
@PreAuthorize("hasAnyRole('ADMIN', 'EDITOR')")
public Document getDocument(Long id) { ... }
```

But for row‑level access (e.g. user requests list of documents), combine with JPA filtering based on permissions as above. This dual‑layer (method + data) is often the most secure and maintainable pattern. Many articles recommend this “hybrid” approach for robust RBAC + data‑security. ([Medium][4])

[1]: https://medium.com/%40victoronu/implementing-role-and-permission-based-authorization-in-spring-boot-with-jwt-359901206b6a?utm_source=chatgpt.com "Implementing Role and Permission-Based Authorization in ..."
[2]: https://www.codejava.net/frameworks/spring-boot/spring-security-jwt-role-based-authorization?utm_source=chatgpt.com "Spring Security JWT Role-based Authorization Tutorial"
[3]: https://stackoverflow.com/questions/66611388/how-to-use-spring-specification-to-filter-through-many-to-many-realtionship?utm_source=chatgpt.com "How to use Spring Specification to filter through many ..."
[4]: https://medium.com/tuanhdotnet/building-a-detailed-role-based-access-control-system-with-spring-security-and-spring-boot-a83ad83a7480?utm_source=chatgpt.com "Building a Detailed Role-Based Access Control System ..."
