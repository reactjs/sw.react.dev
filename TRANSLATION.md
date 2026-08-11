# Mwongozo wa Tafsiri / Translation Style Guide (Swahili · `sw`)

This guide keeps the Swahili translation of react.dev **consistent** across pages and
contributors. Read it before translating, and follow the glossary so every page uses the
same word for the same React concept. Consistency is what makes the docs great to learn from.

Before starting a page, **claim it** on the progress issue
([#1](https://github.com/reactjs/sw.react.dev/issues/1)) so we don't duplicate work. One page
per pull request keeps reviews small.

---

## Golden rules

1. **Translate prose, not code.** Translate the explanatory text. In code blocks, translate
   only `// comments` and clearly user-facing display strings (e.g. button labels, headings
   shown on screen). **Never** rename variables, functions, `props`, imports, or object keys —
   that would break the examples.
2. **Keep heading anchors in English.** A heading like `## Your first component {/*your-first-component*/}`
   becomes `## Component yako ya kwanza {/*your-first-component*/}`. Translate the visible title;
   **leave the `{/*...*/}` slug exactly as-is** — it is the URL anchor and cross-page links depend on it.
3. **Keep MDX components and their props untouched:** `<Sandpack>`, `<Note>`, `<Pitfall>`,
   `<DeepDive>`, `<LearnMore>`, `<YouWillLearn>`, `<Recap>`, `<Challenges>`, `<Diagram>`, `<Intro>`, etc.
   Translate the text *inside* them; never translate the tag names or attributes like `path=` / `name=`.
4. **Keep core API terms in English**, with a short Swahili gloss in parentheses on first use per
   page — e.g. *component (kipengele)*, *state (hali)*. Developers need to recognize the real API
   names. After the first mention on a page, the English term alone is fine.
5. **Don't translate:** JSX/HTML tags and attributes, file paths, URLs, package names, keywords
   (`import`, `export`, `return`, `const`), `console.log` output that code depends on, and proper
   nouns (React, JavaScript, DOM, JSON).
6. **Tone:** clear, friendly, and direct — the same encouraging teaching voice as the English docs.
   Address the reader as *wewe* ("you"). Prefer plain, widely-understood Swahili over rare coinages.

---

## Glossary (Kamusi)

| English            | Swahili convention                        | Notes |
|--------------------|-------------------------------------------|-------|
| component          | **component** *(kipengele)* on first use  | Keep English; homepage/Quick Start use *kipengele* for the gloss. |
| props              | **props**                                 | Keep as-is. |
| state              | **state** *(hali)* on first use           | Keep English; existing pages use *hali*. |
| Hook               | **Hook**                                  | Keep, capitalized. |
| render (verb)      | **ku-render** / kuonyesha                 | Keep "render" as the technical verb; *kuonyesha* (to display) in plain prose. |
| markup             | **markup**                                | Keep as-is. |
| JSX / HTML / CSS / DOM / UI | keep as-is                        | Acronyms stay in English. |
| function           | function *(kitendaji)*                     | Keep English; gloss once if helpful. |
| variable           | kigezo                                     | |
| value              | thamani                                    | |
| array              | array *(safu)*                             | |
| object             | object                                     | Keep as-is. |
| attribute / property | sifa                                     | |
| nested / to nest   | kupachika / kuweka ndani                    | |
| reusable           | inayoweza kutumika tena                     | |
| parent / child (component) | mzazi / mtoto                       | |
| tree               | mti *(tree)*                                | |
| root               | mzizi *(root)*                              | |
| bug                | hitilafu *(bug)*                            | |
| pure function      | pure function *(function safi)*             | |
| import / export    | keep as JS keywords in code; in prose *kuingiza / kutoa* | |
| browser            | kivinjari                                   | |
| library            | maktaba                                     | |
| user interface     | kiolesura cha mtumiaji (UI)                 | |
| to build (an app)  | kuunda / kujenga                            | |
| to declare         | kutangaza                                   | |
| expression         | usemi                                       | |
| operator           | opereta                                     | Prefer *opereta* (not *operesheni*). |
| statement          | kauli                                        | e.g. *kauli ya `if`*. |
| converter          | kigeuzi                                      | |
| module             | module                                       | Keep as-is. |
| node               | nodi                                         | Tree/graph node. |
| bundle / bundler   | bundle / bundler                             | Keep as-is. |
| render tree        | render tree                                  | Keep English; gloss *(mti wa ku-render)* if needed. |
| module dependency tree | module dependency tree                   | Keep English. |
| snapshot           | picha ya mnepo *(snapshot)*                   | |
| side effect        | side effect *(athari za pembeni)*             | Keep English term. |
| mutation / to mutate | mutation *(mabadiliko ya ndani)* / kubadili  | Keep English noun. |
| purity / pure      | pure *(safi)*                                | Keep English. |
| to configure       | kusanidi                                     | |
| to encapsulate     | kufungasha                                   | |
| to refactor        | kuboresha muundo                             | |
| separator          | kitenganishi                                 | |
| suffix / prefix    | kiambishi tamati / kiambishi awali            | |
| expression / templating | usemi / lugha ya templeti                | |

When you introduce a term not in this table, add it here in the same PR so the next contributor
stays consistent.

---

## Frontmatter

Translate the `title:` value. Leave any other frontmatter keys (e.g. `canary`) untouched.

```md
---
title: Component yako ya kwanza
---
```
