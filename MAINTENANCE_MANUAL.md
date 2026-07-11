---
title: Wiki Maintenance Manual
tags: [maintenance, wiki, guide]
updated: "2026-06-18"
---

# Wiki Maintenance Manual

This manual documents the conventions and procedures for maintaining the Obsidian Wiki repository. It covers directory naming rules, the required structure for index files, the workflow for adding, renaming, modifying, and removing content, and a weekly checklist to ensure consistency.

---

## 1. Directory hierarchy & naming rules

- **Two‑digit prefix**: Every top‑level folder and sub‑folder must start with a two‑digit numeric prefix ranging from `00` to `99`. The prefix defines the ordering in the navigation tree.
- **Kebab‑case name**: After the prefix, use a hyphen (`-`) followed by a lower‑case, hyphen‑separated name (kebab‑case). Example: `01-front-end`, `02-back-end`, `15-project-practice`.
- **Full path example**:
  ```
  /wiki/01-front-end/04-typescript/07-advanced/00-Index.md
  ```
- **No spaces or special characters** in folder names other than the hyphen separator. Non‑ASCII characters may be used in file titles but keep folder names ASCII for tooling stability.
- **Depth**: Generally limit nesting to three levels to keep navigation manageable.

---

## 2. `00-Index.md` template and required Front‑Matter

Every folder representing a logical section must contain an `00-Index.md` file that acts as the entry point for that section. The file must follow this exact template:

```markdown
---
title: <Section Title>
tags: [index]
updated: "2026-06-18"
---

# <Section Title>

## 概述
简要描述本章节的范围与目标。

## 目录
- [[子章节1]]
- [[子章节2]]
- ...

*Last updated: 2026-06-18*
```

### Required Front‑Matter fields
| Field   | Description                                   |
|--------|-----------------------------------------------|
| `title`| Human‑readable title of the section (Chinese or English). |
| `tags` | Must include `index` to enable Obsidian dataview queries. |
| `updated`| ISO‑8601 date string of the last modification. |

The front‑matter block must be the very first lines of the file, surrounded by `---` markers.

---

## 3. Step‑by‑step workflow

### 3.1 Adding a new section
1. **Determine the next prefix**: Choose the next unused two‑digit prefix that respects the desired ordering.
2. **Create the folder** using kebab‑case, e.g. `03-new-topic`.
3. **Add `00-Index.md`** inside the new folder using the template above. Fill in the title and a brief overview.
4. **Add child notes** (Markdown files) under the folder as needed.
5. **Update parent index**: Open the parent folder’s `00-Index.md` and add a link to the new folder under the "目录" list.
6. **Commit** the changes.

### 3.2 Renaming a file or folder
1. **Rename the folder/file** while preserving the two‑digit prefix (if the ordering changes, adjust the prefix accordingly). Use `git mv` or the file explorer.
2. **Search for stale links**:
   - Run a repository‑wide search for the old name (`search_files` with content target) to locate any `[[Old‑Name]]` references.
   - Update each occurrence to the new name.
3. **Refresh parent indexes** if the folder name appears in a list.
4. **Commit** the rename and link updates.

### 3.3 Modifying an existing note
1. Edit the markdown file as required.
2. Update the `updated` field in the front‑matter to the current date.
3. If the change affects the section’s outline, adjust the parent `00-Index.md` accordingly.
4. Commit.

### 3.4 Removing a file or folder
1. **Delete the target** (ensure no other notes reference it).
2. **Remove links**: Search for `[[Deleted‑Name]]` across the repo and delete or replace them.
3. **Update parent index**: Remove the entry from the "目录" list in the parent `00-Index.md`.
4. **Commit** the deletion.

---

## 4. Recommended weekly checklist
- [ ] **Index completeness**: Verify that every folder (including nested ones) contains a `00-Index.md` file.
- [ ] **Front‑Matter dates**: Ensure the `updated` field of all `00-Index.md` files reflects the latest modification date.
- [ ] **Broken links**: Run a repository search for `[[` patterns that do not resolve to an existing file (Obsidian will flag them, but you can also grep for `\[\[.*\]\]`).
- [ ] **Prefix ordering**: Confirm that numeric prefixes reflect the intended navigation order; adjust if a new major section was inserted.
- [ ] **Spell‑check titles**: Scan `title:` values for obvious typos or inconsistent capitalization.
- [ ] **Commit hygiene**: Ensure each change is accompanied by a clear commit message referencing the affected section.

---

*Last updated: 2026-06-18*