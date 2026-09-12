<!--
  AKEREM / SYSTEM INDEX
  Custom profile README — intentionally avoids generic stats cards, badge walls,
  fake percentages, and template-heavy portfolio patterns.
-->

<p align="center">
  <img src="./assets/header-dark.svg#gh-dark-mode-only" width="100%" alt="AKEREM — Software Engineer — Java, Spring, PostgreSQL, React" />
  <img src="./assets/header-light.svg#gh-light-mode-only" width="100%" alt="AKEREM — Software Engineer — Java, Spring, PostgreSQL, React" />
</p>

<p align="center">
  <a href="#01--current-work"><code>01 / CURRENT WORK</code></a>&nbsp;&nbsp;·&nbsp;&nbsp;
  <a href="#02--engineering-interests"><code>02 / ENGINEERING INTERESTS</code></a>&nbsp;&nbsp;·&nbsp;&nbsp;
  <a href="#03--system-notes"><code>03 / SYSTEM NOTES</code></a>&nbsp;&nbsp;·&nbsp;&nbsp;
  <a href="#04--recent-output"><code>04 / RECENT OUTPUT</code></a>
</p>

---

## 01 / CURRENT WORK

### FlagTether

**Feature flag management platform** for deterministic rollouts, targeting rules, environment-scoped configuration, and audit history.

`Java 21` · `Spring Boot` · `PostgreSQL` · `React` · `TypeScript` · `Docker` · `Flyway` · `Testcontainers`

[repository →](https://github.com/Akerem-dev/flagtether) · [architecture →](https://github.com/Akerem-dev/flagtether/blob/main/docs/ARCHITECTURE.md) · [release v1.0.1 →](https://github.com/Akerem-dev/flagtether/releases/tag/v1.0.1)

```java
if (feature.isEnabled("new_checkout")) {
    return new CheckoutFlow();
}
```

Built around deterministic evaluation, ordered targeting, transactional audit writes, database constraints, and same-origin deployment.

---

## 02 / ENGINEERING INTERESTS

| index | system area |
| :--- | :--- |
| `01` | backend systems |
| `02` | API design |
| `03` | data integrity |
| `04` | deployment tooling |
| `05` | developer experience |

I prefer systems where correctness is visible in the structure: explicit boundaries, predictable behavior, testable decisions, and infrastructure that stays understandable after the demo is over.

---

## 03 / SYSTEM NOTES

<details open>
<summary><strong>01 — deterministic rollout evaluation</strong></summary>

<br />

FlagTether does not make a random rollout decision on every request. A stable SHA-256 bucket is derived from:

```text
environment:flagName:userKey
```

```text
request
  ↓
flag enabled?
  ↓
targeting rules
  ↓
rollout bucket
  ↓
decision
```

The same exact input produces the same assignment across repeated evaluations.

</details>

<details>
<summary><strong>02 — transactional audit writes</strong></summary>

<br />

Configuration mutations and their matching audit entries are persisted in the same transaction. If the audit write fails, the configuration change does not partially survive.

</details>

<details>
<summary><strong>03 — database-enforced invariants</strong></summary>

<br />

Core domain rules are mirrored at the PostgreSQL boundary through Flyway-managed constraints. Invalid direct writes should not be able to create state the evaluation engine cannot interpret.

</details>

<details>
<summary><strong>04 — same-origin reverse proxying</strong></summary>

<br />

The production-like stack builds the React application into Nginx. Nginx serves the SPA and proxies API, Swagger, and OpenAPI traffic to Spring Boot, keeping internal service names out of the browser.

</details>

---

## 04 / RECENT OUTPUT

```text
2026-09   FlagTether v1.0.1
2026-09   production-like Docker topology
2026-09   PostgreSQL integration coverage
```

The current focus is depth over repository count: one system, clearer boundaries, stronger tests, better deployment behavior, and fewer decorative abstractions.

---

<p align="right">
  <a href="https://github.com/Akerem-dev">github</a>
  &nbsp;·&nbsp;
  <a href="https://github.com/Akerem-dev/flagtether">flagtether</a>
  &nbsp;&nbsp;&nbsp;
  <code>$ keep building.</code>
</p>
