---
title: "Maia Agent Skills"
linktitle: "Agent Skills"
url: /refguide/maia-agent-skills/
weight: 88
description: "Describes how to create and manage agent skills that equip Maia with domain-specific knowledge, applied automatically whenever relevant."
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

{{% alert color="info" %}}
This feature is released as part of [Maia Make](/refguide/maia-make/) capabilities in Studio Pro 11.11 and above.

To use agent skills, an internet connection and signing in to Studio Pro are required.
{{% /alert %}}

Agent skills are modular, reusable instructions that extend Maia's capabilities with domain-specific knowledge—giving it the context, workflows, and guidance it needs to work the way your team works.

Instead of typing the same context into every chat, you define agent skills once and Maia applies them automatically whenever relevant, eliminating the need to repeat the same guidance across conversations.

Use agent skills to equip Maia with the domain knowledge and conventions it needs—whether that is company-wide standards or team-specific workflows. For example, a skill capturing your company's Mendix naming conventions means Maia applies them consistently whenever it generates entities, attributes, or microflows—without you having to mention them each time.

For guidance on writing effective skills, see [Best Practices](https://agentskills.io/skill-creation/best-practices) and [Optimizing Descriptions](https://agentskills.io/skill-creation/optimizing-descriptions) in the [Agent Skills documentation](https://agentskills.io/).

## How Agent Skills Work

At the start of each chat session, Maia becomes aware of all agent skills in your project. As you work, Maia picks up the relevant skill content automatically—you do not need to reference or invoke skills explicitly.

If you add or edit a skill while a session is active, those changes take effect the next time you start a chat.

For more on how skills load and when they apply, see the [Agent Skills specification](https://agentskills.io/specification).

## Directory Structure {#directory-structure}

Skills are stored in a `skillssource/` directory at the root of your app directory. You can also manage skills directly in the file system—for example, to copy in skills from another project. After making changes in the file system, go to **App** > **Synchronize App Directory** (shortcut: <kbd>F4</kbd>) to make the changes visible in Studio Pro.

```
skillssource/
  your-skill/
    SKILL.md
    references/
      GLOSSARY.md
      NOTES.md
```

### SKILL.md Format {#skill-md-format}

Each `SKILL.md` file must include YAML frontmatter at the top, followed by the skill content in Markdown:

```yaml
---
name: your-skill
description: Applies company-wide naming conventions for entities, attributes, and microflows.
---

Write your skill instructions here in plain Markdown. Use the `references/` subdirectory
to add supplementary content and refer to it from here as needed.

See [my-reference](references/my-reference.md) for details.
```

| Field | Required | Description |
| --- | --- | --- |
| `name` | Yes | Must match the skill's parent directory name exactly. See the [Agent Skills specification](https://agentskills.io/specification#name-field) for full naming rules. |
| `description` | Yes | See the [Agent Skills specification](https://agentskills.io/specification#description-field). |

The `name` field must match the skill's parent directory name exactly—a mismatch prevents the skill from loading. For example, if the skill directory is `skillssource/your-skill/`, the `name` field must be `your-skill`.

## Creating a Skill {#creating-a-skill}

To create a new agent skill:

1. In the **App Explorer**, expand **Maia** > **skills**.
2. Right-click **skills** and click **Add** > **Skill**.
3. Enter a name for the skill.

Studio Pro creates the new skill. Open the skill from the **App Explorer** to add your content.

## Adding Reference Files {#reference-files}

Reference files let you keep your skill focused by moving supplementary content into separate documents—a glossary of domain terms, a data dictionary, or detailed architecture notes, for example. Maia accesses reference file content on demand, only when relevant to your request.

To add a reference file to a skill:

1. In the **App Explorer**, under **Maia** > **skills**, right-click the skill you want to add a reference to.
2. Click **Add** > **Reference**.
3. Enter a name for the reference file.

For more on how skills and reference files load, see the [Agent Skills specification](https://agentskills.io/specification#progressive-disclosure).

## Limitations {#limitations}

* Only Markdown files are supported as reference files. Files in other formats are ignored.
* Changes you make to skills take effect the next time you start a chat.

## Read More

* [Maia Make Capabilities](/refguide/maia-make/)
* [Mendix AI Assistance (Maia)](/refguide/mendix-ai-assistance/)
* [Maia Chat](/refguide/maia-chat/)
* [Agent Skills Best Practices](https://agentskills.io/skill-creation/best-practices) – guidance on writing effective skill content
* [Agent Skills Specification](https://agentskills.io/specification) – the full specification for the agent skills format
