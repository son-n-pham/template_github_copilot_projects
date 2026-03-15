---
name: plan
description: 'Manage hierarchical planning files (Vision → Feature → BDD → TDD) with ID-based traceability. Use when: creating a project plan, adding features, writing BDD scenarios, scaffolding TDD tests, updating a tree map, processing user input files, or maintaining a vision-to-test traceability chain.'
argument-hint: 'Describe the planning action (e.g., "create feature V1-F2", "generate BDD for V1-F1", "update tree map")'
---

# Plan Skill

Manages a hierarchical system of Vision, Feature, BDD, and TDD files with strict ID-based traceability. Every file is linked to its parent, forming an unbroken chain from vision to test.

## ID Structure

| Level   | Pattern           | Example         |
|---------|-------------------|-----------------|
| Vision  | `V{n}`            | `V1`            |
| Feature | `V{n}-F{n}`       | `V1-F1`         |
| BDD     | `V{n}-F{n}-B{n}`  | `V1-F1-B1`      |
| TDD     | `V{n}-F{n}-B{n}-T{n}` | `V1-F1-B1-T1` |

Rules:
- IDs are unique across the project.
- A child ID must include the full parent chain (no orphan IDs).
- Increment the rightmost counter for each new sibling.

## File Layout

```
docs/plan/
├── vision/
│   ├── V1.md
│   └── V1_user.md
├── features/
│   ├── V1-F1.md
│   └── V1-F1_user.md
├── bdd/
│   ├── V1-F1-B1.md
│   └── V1-F1-B1_user.md
├── tdd/
│   ├── V1-F1-B1-T1.md
│   └── V1-F1-B1-T1_user.md
└── tree-map.md
```

## Procedure

### 1. Determine Action

Identify one of these actions from the user's request:

| Action | Trigger |
|--------|---------|
| **Create** | "create", "add", "new" + level noun |
| **Update** | "update", "change", "edit" + file ID |
| **Process user input** | "_user" file exists with unprocessed entries |
| **Generate children** | "generate BDD/TDD for {ID}" |
| **Sync tree map** | "update tree map", or after any create/update |

### 2. Create a File

1. Determine the correct ID by scanning existing files at the target level.
2. Select the appropriate template from `./assets/`:
   - Vision → [vision.md](./assets/vision.md)
   - Feature → [feature.md](./assets/feature.md)
   - BDD → [bdd.md](./assets/bdd.md)
   - TDD → [tdd.md](./assets/tdd.md)
3. Fill in the ID, Status (`New`), Description, and parent Links.
4. Save to the correct directory (e.g., `docs/plan/features/V1-F2.md`).
5. Update `docs/plan/tree-map.md`.

### 3. Process User Input

User input files share the same name as the agent file but end in `_user` (e.g., `V1-F1_user.md`). Template: [user-input.md](./assets/user-input.md).

Steps:
1. Find all `*_user.md` files that contain unprocessed entries (entries without an `Applied:` timestamp).
2. For each entry, compare the requested change against the corresponding agent file.
3. Apply the change: update the Description, add/modify content, or change Status.
4. Stamp the user entry with `Applied: {ISO 8601 timestamp}`.
5. If the user input requests a new child file (e.g., "add BDD for this feature"), generate that file using the template and correct ID.
6. Update the tree map.

### 4. Generate Missing Children

When asked to generate children for an ID (e.g., "generate BDD for V1-F1"):
1. Read the parent file to understand scope.
2. Determine the next available child ID.
3. Create all missing child files using the appropriate template.
4. Update the tree map.

### 5. Update the Tree Map

After any create or update action, regenerate `docs/plan/tree-map.md` using [tree-map.md](./assets/tree-map.md) as the format reference.

- Reflect the full hierarchy: Vision → Features → BDD → TDD.
- Each node shows its ID, Status, and a relative file link.
- Completed/archived items may be collapsed under an `## Archive` section.

### 6. Archive Completed Work

When a file's Status becomes `Completed`:
1. Move it to `docs/plan/archive/{level}/{ID}.md`.
2. Mark the corresponding `_user` entries as `Archived`.
3. Remove from the active tree map and add to the archive section.

## Status Values

| Status      | Meaning                              |
|-------------|--------------------------------------|
| `New`       | Created, not yet started             |
| `In Progress` | Actively being worked on           |
| `Completed` | Done, ready for archiving            |
| `Archived`  | Moved out of active files            |
| `Blocked`   | Waiting on a dependency              |

## Quality Checks

Before finishing any action, verify:
- [ ] ID follows the correct parent-child chain.
- [ ] All Links fields reference valid parent IDs.
- [ ] No duplicate IDs exist at the same level.
- [ ] Tree map reflects the change.
- [ ] User input entries are stamped `Applied` after processing.
