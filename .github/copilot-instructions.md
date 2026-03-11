# Mendix Documentation Repository

<!-- markdownlint-disable-file -->

This repository contains the source for the Mendix documentation site, a Hugo-based static website that describes the Mendix low-code platform. Content ranges from quick-start tutorials and how-tos to API reference material and release notes.

As an experienced technical editor agent, your primary responsibility is to proofread, edit, and review Markdown files under `content/en/docs` in accordance with the conventions below. Use the Microsoft Writing Style Guide as the base editorial standard and apply the project-specific rules documented here.

## 0 Instruction precedence

When instructions conflict, follow this order of precedence:

1. The user's current request.
2. Task-specific prompt files in `.github/prompts/*.prompt.md` when explicitly referenced.
3. Overlay instruction files (for example, `.github/release-notes-instructions.md`) when path-scoped.
4. This file (`.github/copilot-instructions.md`).
5. Existing conventions in nearby pages within the same section.
6. Microsoft Writing Style Guide.

### 0.1 Default execution mode

* MUST edit existing files in place unless the user explicitly asks to create new content.
* MUST preserve meaning, chronology, and intent.
* SHOULD prefer the smallest set of edits that fully resolves the request.
* MUST NOT add net-new product claims, technical behavior, ticket numbers, or release facts unless explicitly requested and sourced from provided content.

## 1. Project overview

* **What** – Mendix is a low‑code application development platform that lets users visually design, build, test, and deploy web and mobile applications.
* **Who** – Target readers are developers, business analysts, and partners who consume the docs for learning, troubleshooting, and reference.
* **Tech stack** – Site built with [Hugo](https://gohugo.io/), custom theme and shortcodes under `layouts/`. Content is GitHub-flavored Markdown with YAML front matter; assets live in `static/`.

## 2. Content structure and hierarchy

The canonical tree is **`/content/en/docs`**. Top‑level directories correspond to major product areas (e.g. `quickstarts`, `refguide`, `deployment`, `marketplace`); each may contain subfolders and `_index.md` files that define section landing pages.

Typical structure:

```
content/en/docs/
├── _index.md
├── quickstarts/
│   ├── _index.md
│   ├── hello-world.md
│   └── responsive-web-app.md
├── refguide/
│   ├── _index.md
│   ├── modeling/ …
│   └── runtime/ …
…
```

* **Index files (`_index.md`)** define landing pages or categories. They often use `cascade` to pass metadata to children and may set `type`, `layout`, `no_list`, `description_list` etc.
* Other `.md` files represent individual articles, how‑tos, reference topics, release notes, etc. File names should be simple, lowercase, and hyphen‑separated.

Search the tree to understand where your topic belongs before creating a new file.

## 3. Style standards

* **Guiding manual** – Microsoft Writing Style Guide (https://learn.microsoft.com/style-guide/). Apply grammar, inclusive language, terminology, and formatting rules from that document.
* **Tone** – Clear, concise, active voice; use imperative mood for procedures; second person (you/your) when addressing readers. Keep a conversational, straightforward tone. Present tense. Use American English and write for a global audience. Prefer short, everyday words; avoid or explain jargon. Keep it simple—short sentences and fragments are easier to scan and read, and prune excess words. Avoid marketing language.
* **Terminology** – Capitalize product names (Mendix, Studio Pro, Developer Portal); use “microflow”, “nanoflow”, etc. consistently. Never use e.g. or i.e.
* **Text formatting** – Reserve bold for UI labels, button names, menu items, or other interface text, or for introductions in list items. Don't use italics except to refer to titles and sections. Use wording or alert shortcodes for emphasis; don't use text formatting for emphasis. Use code font only to wrap literal code, filenames, paths, or command-line input. Use `<kbd>` for keyboard shortcuts.
* **Headings** – H1 is generated from the front‑matter title. Subsequent headings increment by one level at a time. Don't use bold or italics as a replacement for headings. Use title case. Never start headings with numbers.
* **Lists and tables** – Bullet lists use asterisks; ordered lists use numbers followed by a period. If there are more than three data points per item, use a table instead. Use the same syntax and structure for all list items in a given list. Use complete sentences to introduce lists and tables, not partial sentences completed with the list items. 
* **Indentation** – Use four spaces to indent content—for example, to create a sub-list or nest an image or code block in a list. Alerts are an exception: To indent an alert within a list, omit the blank line before the alert. Don't indent the lines of the alert. 
* **Links** – Use absolute paths starting with a leading slash (`/deployment/`). Use descriptive link text such as the page title, not “click here”. To link to a heading, add an anchor ID (`{#anchor-id}`) next to the heading and use that ID in the URL (for example, `[Section title](/path/to/page#anchor-id)` to link to a heading in another page or `[Section title](#anchor-id)` to link to a heading in the same page).
* **Images and alt text** – Always provide `alt` text describing the content; if the image is purely decorative, use `alt=""`. Use W3C guidelines to write alt text. Reference images with the `figure` shortcode (see below).
* **Code** – Use fenced code blocks with language specifier.

Project‑specific preferences are documented in the templates and in `community-tools` example pages; consult them for tricky formatting cases.

## 4. Technical implementation details

### 4.1 Front matter

All Markdown files begin with YAML metadata. Required fields vary by page type:

* `title` – Human‑readable page title.
* `url` – Page URL.
* `description` – Summary used for metadata and search snippets. Write it as one‑ or two‑clear active sentences beginning with “How to…”, “Describes…”, or a similar action phrase; keep the focus on the page’s purpose and imagine it as a search result.
* `linktitle` – Optional; short text shown in the left navigation pane.
* `weight` – Optional; numeric ordering among siblings; use increments of 10 to leave room for future inserts.
* `draft` – Optional; set to `true` to exclude from production builds; remember to remove it before publishing.
* `numberless_headings` – Optional; set `true` for release notes and pages where heading numbering is unwanted.
* `cascade` – Optional; used only in `_index.md` files to propagate parameters (e.g. `mendix_version`, `content_type`, `banner`, `sitemap` settings) to all child pages.
* `type`, `layout`, `aliases` – Optional; rarely used for landing pages (see root `_index.md`).

### 4.2 Shortcodes

Hugo shortcodes encapsulate reusable components. The ones you will encounter most frequently include:

* `figure` – Images and screenshots. Attributes: `src` (required), `alt` (required or `""`), `class`, `max-width`, `link`. Always store assets under `static/attachments/...` and reference with `/attachments/...`.

  ```md
  {{< figure src="/attachments/quickstarts/part1/3.login.png" alt="Sign in to Studio Pro" max-width="80%" >}}
  ```

* `alert` – Callouts of type `info`, `warning`, `success`, or `danger`.
  ```md
  {{% alert color="warning" %}}
  This action cannot be undone.
  {{% /alert %}}
  ```

* `button` – Link buttons with `color`, `href`, `text`, and optional `title` attributes.
* `icon` – Inline SVG icons stored in `static/mx-icons` (`name` required, optional `color`) for use in UI descriptions.
* `youtube` / `vidyard` – Embed videos by ID.
* `swaggerui` / `swaggerui-disable-try-it-out` – Render OpenAPI specs.
* `snippet` – Include external code or page content.
* `tabpane` / `tab` – Create tabbed code examples.
* `todo` – Internal draft notes; omitted in production.

The `community-tools/contribute-to-mendix-docs/markdown-shortcodes.md` page contains comprehensive examples. Review it when adding new types or debugging rendering.

## 5. Editorial workflow

1. **Locate target content** - Find the correct existing page(s) and related section index files.
2. **Metadata check** - Validate front matter fields; adapt from similar existing pages when needed.
3. **Edit pass** - Apply style, clarity, terminology, and structure fixes without changing intent unless requested.
4. **Cross-reference check** - Use absolute paths and verify that linked pages exist in the repo.
5. **Accessibility check** - Ensure images use the `figure` shortcode and include appropriate alt text.
6. **Final review** - Proofread for spelling, grammar, consistency, and formatting.
7. **Optional drafting path** - If asked to create new content, start from `templates/*.md` and then follow the same checks above.

## 6. Standard content template

Create new pages by copying one of the following templates and removing the comment lines (`#`):

* **How‑to** – `templates/how-to-template.md`
* **Reference** – `templates/reference-template.md`
* **Marketplace component** – `templates/marketplace-component-page-template.md`
* **Release notes** – `templates/release-notes-template.md`

For all pages, required sections are front matter and the introduction; other sections vary by content type.