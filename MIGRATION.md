# Spring Boot 2 → 3 Migration + Code Standardization via OpenRewrite

## Summary

- Migration from **Java 8 → 17** and **Spring Boot 2.1.8 → 3.0.13**
- Code standardization applied on top of migration
- **51 files** modified, project compiles successfully
- Only manual change: `SecurityConfig` rewrite (`WebSecurityConfigurerAdapter` removal)

## Recipes used

### Migration
- `org.openrewrite.java.migrate.UpgradeToJava17`
- `org.openrewrite.java.spring.boot3.UpgradeSpringBoot_3_0`

### Standardization
- `org.openrewrite.staticanalysis.CommonStaticAnalysis`
- `org.openrewrite.java.OrderImports`
- `org.openrewrite.java.RemoveUnusedImports`
- `org.openrewrite.java.format.AutoFormat`
- `org.openrewrite.java.spring.NoAutowiredOnConstructor`
- `org.openrewrite.java.spring.framework.BeanMethodsNotPublic`

## What OpenRewrite did automatically

| Category | Change | Impact |
|---|---|---|
| **Migration** | `javax.*` → `jakarta.*` | 164 imports across 34 files |
| **Migration** | `antMatchers()` → `requestMatchers()` | SecurityConfig |
| **Migration** | Security `.and()` chain → lambdas | SecurityConfig |
| **Migration** | `@EnableGlobalMethodSecurity` → `@EnableMethodSecurity` | SecurityConfig |
| **Migration** | `String.format()` → `.formatted()` | 3 occurrences |
| **Migration** | `@Serial` on `serialVersionUID` | 12 classes |
| **Migration** | Spring Boot parent `2.1.8` → `3.0.13` | pom.xml |
| **Migration** | `mysql-connector-java` → `mysql-connector-j` | pom.xml |
| **Migration** | `mockito-all` → `mockito-core` | pom.xml |
| **Migration** | `javax.xml.bind` → `jakarta.xml.bind` | pom.xml |
| **Migration** | Lombok `1.18.12` → `1.18.44` | pom.xml |
| **Migration** | Added `spring-boot-starter-validation`, `jakarta.servlet-api` | pom.xml |
| **Standardization** | `@Bean public` → `@Bean` package-private | 6 methods |
| **Standardization** | Import ordering (jakarta before org/spring) | 6 files |
| **Standardization** | Unused imports removed | 4 files |
| **Standardization** | Inconsistent indentation fixed | 3 files |
| **Standardization** | Added `final` to immutable fields | UserPrincipal |
| **Standardization** | Added `{}` to single-line if/else | UserPrincipal |
| **Standardization** | Removed unnecessary spaces in annotations | 3 files |

## What was done manually

- `SecurityConfig`: removed `WebSecurityConfigurerAdapter`, created `SecurityFilterChain`, `DaoAuthenticationProvider`, and `AuthenticationManager` beans

## What still needs manual migration

- jjwt `0.9.1` → `0.12.x` API (no OpenRewrite recipe available)

## Validation

```bash
mvn -q -DskipTests compile  # ✅ compiles successfully
```
