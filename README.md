<!--
  AKEREM / SYSTEM INDEX
  Personal engineering profile.
-->

<p align="center">
  <img src="./assets/header-dark.svg#gh-dark-mode-only"
       width="100%"
       alt="AKEREM — Software Engineer — Java, Spring, PostgreSQL, React" />

  <img src="./assets/header-light.svg#gh-light-mode-only"
       width="100%"
       alt="AKEREM — Software Engineer — Java, Spring, PostgreSQL, React" />
</p>

<p align="center">
  <a href="#01--current-work"><code>01 / CURRENT WORK</code></a>
  &nbsp;·&nbsp;
  <a href="#02--engineering-focus"><code>02 / ENGINEERING FOCUS</code></a>
  &nbsp;·&nbsp;
  <a href="#03--system-notes"><code>03 / SYSTEM NOTES</code></a>
  &nbsp;·&nbsp;
  <a href="#04--recent-output"><code>04 / RECENT OUTPUT</code></a>
</p>

---

## 01 / CURRENT WORK

### FlagTether

**Feature flag management platform** built around deterministic rollouts, targeting rules, environment-scoped configuration, and auditable changes.

`Java 21` · `Spring Boot` · `PostgreSQL` · `React` · `TypeScript` · `Docker` · `Flyway` · `Testcontainers`

[repository →](https://github.com/Akerem-dev/flagtether)
&nbsp;·&nbsp;
[architecture →](https://github.com/Akerem-dev/flagtether/blob/main/docs/ARCHITECTURE.md)
&nbsp;·&nbsp;
[release v1.0.1 →](https://github.com/Akerem-dev/flagtether/releases/tag/v1.0.1)

```java
if (feature.isEnabled("new_checkout")) {
    return new CheckoutFlow();
}
```

The toggle is the easy part.

The interesting work is everything around it: deterministic evaluation, ordered targeting, transaction-safe audit writes, database-enforced constraints, real PostgreSQL integration testing, and a production-like same-origin deployment.

<details>
<summary><strong>inspect the evaluation path</strong></summary>

<br />

```text
request
  │
  ▼
load environment flag
  │
  ▼
globally enabled?
  │
  ├── no ──> FLAG_DISABLED
  │
  ▼
ordered targeting rules
  │
  ├── match ──> TARGETING_MATCH
  │
  ▼
percentage rollout
  │
  ▼
SHA-256 deterministic bucket
  │
  ├── inside rollout ──> ROLLOUT_MATCH
  │
  └── outside rollout ─> ROLLOUT_MISS
```

</details>

---

## 02 / ENGINEERING FOCUS

<table>
<tr>
<td width="50%" valign="top">

### Systems I like working on

`01` backend systems  
`02` API design  
`03` data integrity  
`04` deployment tooling  
`05` developer experience

</td>
<td width="50%" valign="top">

### Things I care about

`01` predictable behavior  
`02` explicit boundaries  
`03` useful tests  
`04` boring reliability  
`05` understandable systems

</td>
</tr>
</table>

I prefer software where correctness is visible in the structure.

Fewer decorative abstractions.  
Clearer ownership.  
Failures that are understandable.  
Infrastructure that still makes sense after the demo is over.

---

## 03 / SYSTEM NOTES

Small notes from decisions that mattered while building.

<details open>
<summary><strong>01 — deterministic rollout evaluation</strong></summary>

<br />

Percentage rollout is not random on every request.

FlagTether derives a stable SHA-256 bucket from:

```text
environment:flagName:userKey
```

The same exact input produces the same assignment across repeated evaluations.

That makes rollout behavior predictable without storing an assignment for every user.

</details>

<details>
<summary><strong>02 — targeting runs before rollout</strong></summary>

<br />

Explicit targeting rules are evaluated before percentage rollout.

This lets a specific rule override the general rollout percentage without making the rollout algorithm itself more complicated.

```text
targeting decision
        ↓
no explicit match
        ↓
percentage rollout
```

</details>

<details>
<summary><strong>03 — configuration and audit belong together</strong></summary>

<br />

Configuration mutations and their matching audit records are persisted in the same transaction.

If the audit write fails, the configuration change should not partially survive.

```text
configuration write
        +
audit write
        │
        ▼
   one transaction
```

</details>

<details>
<summary><strong>04 — database invariants are not optional</strong></summary>

<br />

Important domain rules are mirrored at the PostgreSQL boundary through Flyway-managed constraints.

Application validation improves the API experience.

Database constraints protect the data itself.

I want both.

</details>

<details>
<summary><strong>05 — test against the database you actually use</strong></summary>

<br />

Integration tests run against real PostgreSQL through Testcontainers rather than replacing production behavior with an in-memory database.

That means migrations, SQL, constraints, and transaction semantics are exercised against the same database engine used by the application.

</details>

<details>
<summary><strong>06 — same-origin deployment keeps the browser simple</strong></summary>

<br />

The production-like setup builds the React frontend into Nginx.

```text
browser
   │
   ▼
 nginx
 ├──────── static React
 │
 └──────── /api
             │
             ▼
        Spring Boot
             │
             ▼
        PostgreSQL
```

The browser sees one origin while internal services remain internal.

</details>

---

## 04 / RECENT OUTPUT

```text
2026-09   FlagTether v1.0.1
2026-09   project-wide identifier cleanup
2026-09   production-like Docker topology validation
2026-09   PostgreSQL integration coverage
2026-09   portfolio presentation pass
```

Current direction:

```text
depth        > repository count
clarity      > cleverness
correctness  > happy-path demos
shipping     > endless abstraction
```

---

<details>
<summary><strong>operator notes</strong></summary>

<br />

```text
currently exploring   backend architecture
usually debugging     something I was sure worked
preferred database    PostgreSQL
favorite outcome      boring production behavior
status                still building
```

</details>

---

<p align="right">
  <a href="https://github.com/Akerem-dev">github</a>
  &nbsp;·&nbsp;
  <a href="https://github.com/Akerem-dev/flagtether">flagtether</a>
  &nbsp;&nbsp;&nbsp;
  <code>$ keep building.</code>
</p>
