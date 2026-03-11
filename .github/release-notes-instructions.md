---
applyTo:
  - "content/en/docs/releasenotes/**/*.md"
---

# Release Notes Instructions

<!-- markdownlint-disable-file -->

## Essentials

### Page Layout

Use the existing template in `templates/release-notes-template.md` as a reference and ensure required fields are present and accurate.

* Use `## <version> {#anchor}` for each release, with a release date and optional download button.
* Allowed section headings: `New Features`, `Improvements`, `Fixes`, `Deprecations`, `Limitations`, `Breaking Changes`, `Known Issues`.
* Use `<a id="123456"></a>` before important fixes; cross-link with `[#123456](#123456)`.
* Add a `{{% button ... %}}` to Marketplace or download pages, if relevant.
* Use `{{% alert %}}` for critical notices.

### Tone and Content

* Accurate, factual, and concise; active voice.
* Include only customer-visible changes; omit internal notes.
* Include instructions or workarounds briefly, with links.
* Avoid marketing language; use images rarely.

### Formatting Rules

* New entries newest-first.
* Follow existing filename and styling patterns.
* Use a separate file per minor release unless grouping already exists.
* Prefix subsystem-specific notes (for example, "Improvements to SAP Deployment").
* Append `(Ticket 123456)` where applicable; list ticketed items first, in ascending order.

### Validation Workflow

* Treat this as an edit-and-check workflow for existing release-note content.
* Validate structure first (front matter, heading levels, allowed section names), then normalize formatting and check for typos.
* Keep meaning unchanged unless explicitly asked to rewrite content.
* Don't add new claims, ticket IDs, or release details that are not already present. If something is missing, flag it in the chat.

### Cross-References

Use absolute paths for docs; use anchors or full URLs for other release notes.

This guidance is a lightweight overlay over the main Copilot instructions.

## Release Note Types

Use the following table as an allowlist and quality reference when writing notes or checking existing notes.

| Release Note Type | Definition | What Good Looks Like | Example |
| --- | --- | --- | --- |
| New Features | A major new feature that customers can see and use. | Name the feature and briefly describe it, including the user benefit. | We added a new feature that allows you to reverse an association direction by right-clicking the association and selecting **Reverse direction**. |
| Improvements | A functional fix or minor feature that makes behavior more consistent and predictable for customers. | State what changed and describe the user benefit of the new behavior. | Studio Pro now detects when an external date and time attribute that represents only a date has been localized and gives a consistency error. |
| Fixes | A technical bug or error fix, when reported by customers or when behavior or flow changed. | Preferred pattern starts with `We fixed an issue where...` and briefly describes where the bug occurred. If a customer reported it, include the ticket number. For Mendix-specific security vulnerabilities (CVEs), consult the Information Security team before publishing. | We fixed an issue where the bottom navigation bar invaded the Safe Area view on iPhone devices. (Ticket 187402) |
| Deprecations | Functionality that is no longer available, or that is deprecated and will be removed in a future version. | If removed, start with `We removed...` or `We dropped support for...`. If deprecated but not yet removed, start with `We deprecated...` and state when it will be removed. | We dropped support for PostgreSQL 11, as it is no longer supported by the vendor. |
| Known Issues | Confirmed unresolved issues that can affect users. May include workarounds. | Describe impact clearly and include workaround details when available. | For some activities in the logic editors, variable types are not visible or are shown as **Not set**. This is only a visual bug. (Tickets 205751, 207251) |
| Limitations | Known product constraints that are not necessarily defects. | State the limitation directly and explain scope. | This feature is available only for apps running in cloud-connected environments. |
| Breaking Changes | Confirmed changes that require user action or can break existing behavior. | State what changed, impact, and required user action. | We changed the default API endpoint format. Update existing integrations to use the new endpoint pattern before upgrading. |

### Anchors and Ticket Links

In the **Fixes** and **Known Issues** sections, include an HTML anchor for important tickets so readers can jump directly from other notes. Use the `<a id="123456"></a>` pattern immediately before the bullet item. Later, cross-link using `[#123456](#123456)` or by referencing the heading.

### Alerts and Callouts

Use the `{{% alert %}}` shortcode for important notices such as blog post links, upgrade warnings, or temporary workarounds. Example:

```md
{{% alert color="info" %}}
For more information on this release, see the [Mendix 8.15 blog post](/blog/…).
{{% /alert %}}
```

### Cross-Referencing

Always link to relevant documentation (refguide, how-to pages) using absolute paths. When referring to other release-note versions, use relative anchors or full URLs (for example, `/releasenotes/studio-pro/8.17/#875726`). Keep links descriptive and avoid vague text such as "see here."

### Additional Notes

* Keep text concise and technically focused; do not introduce marketing language.
* Refer to prior examples in the `releasenotes` tree when in doubt.