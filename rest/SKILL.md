# REST API Design Guide

This skill defines the standards and conventions for designing and implementing REST APIs in the ACCM project.

## 🗂️ Resource Naming

- Use **nouns**, never verbs: `/api/v1/people`, not `/api/v1/getUsers`
- Use **plural** for collections: `/api/v1/people`
- Use **singular** as the domain term for a single item (e.g. *person* is the domain name for an item of *people*)
- Version all APIs under a path prefix: `/api/v1/`
- Use **kebab-case** for multi-word resources: `/api/v1/comic-books`

## 🔁 HTTP Methods & Status Codes

| Operation       | Method   | Path                  | Success code  |
|-----------------|----------|-----------------------|---------------|
| List            | `GET`    | `/api/v1/resources`   | `200 OK`      |
| Get one         | `GET`    | `/api/v1/resources/{id}` | `200 OK`   |
| Create          | `POST`   | `/api/v1/resources`   | `201 Created` |
| Full update     | `PUT`    | `/api/v1/resources/{id}` | `200 OK`   |
| Partial update  | `PATCH`  | `/api/v1/resources/{id}` | `200 OK`   |
| Delete          | `DELETE` | `/api/v1/resources/{id}` | `204 No Content` |

## 📦 DTO Rules

### One DTO per resource
Always use the **same DTO class for all operations** (input and output) on a given resource.

- ✅ One `PersonDto` used for `POST`, `PUT`, `GET`, list, and response
- ❌ Never create separate `CreatePersonRequest`, `UpdatePersonRequest`, `PersonResponse`

### Controlling field visibility
Use Jackson annotations to control what is readable/writable without creating multiple DTOs:

```java
public record PersonDto(
    UUID id,                        // ignored on input (set by server)
    @NotBlank String firstname,
    @NotBlank @Email String email,
    PersonStatus status,            // ignored on input (set by server)
    @JsonProperty(access = JsonProperty.Access.WRITE_ONLY) String password  // never returned
) {
    public static PersonDto from(Person person) { ... }
}
```

- `@JsonProperty(access = WRITE_ONLY)` — accepted on deserialization, never serialized (e.g. password)
- `@JsonProperty(access = READ_ONLY)` — serialized in responses, ignored on input (e.g. id, status, createdAt)

### Validation
Place Bean Validation annotations (`@NotBlank`, `@Email`, `@NotNull`, etc.) directly on the DTO record.
They are only enforced when `@Valid` is present on the controller method parameter.

## 🛡️ Error Responses

Use Spring's `ProblemDetail` (RFC 7807) for all error responses.

```java
@ExceptionHandler(NoSuchElementException.class)
public ProblemDetail handleNotFound(NoSuchElementException ex) {
    ProblemDetail problem = ProblemDetail.forStatusAndDetail(HttpStatus.NOT_FOUND, ex.getMessage());
    problem.setTitle("Resource Not Found");
    return problem;
}
```

Standard error shape:
```json
{
  "type": "about:blank",
  "title": "Resource Not Found",
  "status": 404,
  "detail": "Person not found: 00000000-0000-0000-0000-000000000001"
}
```

## 📖 OpenAPI Documentation

Every REST module must expose OpenAPI documentation via `springdoc-openapi`:

```yaml
springdoc:
  api-docs:
    path: /api-docs
  swagger-ui:
    path: /swagger-ui.html
```

- Annotate controllers with `@Tag(name = "...", description = "...")`
- Annotate each endpoint with `@Operation(summary = "...")`
- Document error responses with `@ApiResponse`

## ✅ Checklist before creating a new REST resource

- [ ] Resource name is plural, kebab-case, versioned under `/api/v1/`
- [ ] Single DTO per resource, used for both input and output
- [ ] `@JsonProperty(access = WRITE_ONLY / READ_ONLY)` applied where fields differ between request and response
- [ ] Bean Validation annotations on DTO fields
- [ ] `@Valid` on all controller method parameters that accept a request body
- [ ] Soft delete implemented as a status change, never a physical row deletion
- [ ] ProblemDetail used for all error responses
- [ ] OpenAPI `@Tag` and `@Operation` annotations present
- [ ] Flyway migration provided for any new table
