# Command Notes

A personal, curated collection of command-line reference pages.  
Adapted from the original [tldr-pages](https://github.com/tldr-pages/tldr) project, but trimmed down to the platforms I care about most.

---

## 📂 Structure

This repo contains 5 folders of command reference pages:

- `android/` → Commands for Android debugging and tools.
- `cisco-ios/` → Cisco IOS networking commands.
- `common/` → Cross-platform commands used on most systems (e.g. `ls`, `tar`).
- `linux/` → Linux-specific commands.
- `windows/` → Windows/PowerShell commands.

Each file is a short **Markdown (`.md`) cheatsheet** for one command.  
Example page structure:

```md
# command

> One-line description.
> More info: <https://example.com>

- Example description:

`command --option value`
```

---

## 📝 Formatting rules

This repo enforces a simple Markdown style:

- Headings must use `#` (ATX style).
- Lists indent with 4 spaces.
- Lines can be long (up to 250 chars).
- Placeholders are written like `{{this}}`.
- No italics/bold styling — keep it clean.

These rules are enforced automatically by **[markdownlint](https://github.com/DavidAnson/markdownlint)** (see `.markdownlint.json`).

---

## 🔍 How to use this repo

- **Search in VS Code** → `Ctrl+Shift+F`, type the command (e.g. `scp`).  
- **Browse by folder** → open `common/`, `linux/`, etc.  
- **Preview Markdown** → in VS Code, right-click a file → “Open Preview” (or `Ctrl+Shift+V`).  

Optional: lint pages locally with [tldr-lint](https://github.com/tldr-pages/tldr-lint):

```bash
npm install -g tldr-lint
tldr-lint pages/common/ls.md
```

---

## 🖊️ Git workflow reminders

Basic workflow for making changes:

```bash
# Stage all changes
git add -A

# Commit with a message
git commit -m "update notes"

# Push to GitHub
git push
```

Check status:

```bash
git status
```

View history (compact form):

```bash
git log --oneline --decorate --graph --all
```

---

## 📖 Markdown reminders

Markdown = plain text + light formatting.  
Quick syntax guide:

- Heading: `# H1`, `## H2`, etc.  
- List:
  ```md
  - Item 1
      - Nested item
  ```
- Code block: triple backticks  
  \`\`\`bash  
  ls -la  
  \`\`\`  
- Inline code: `` `ls` ``  
- Link: `<https://example.com>`  
- Placeholder values: `{{path/to/file}}`

Use **Preview in VS Code** (`Ctrl+Shift+V`) to see formatted output.

---

## 🚀 Why GitHub instead of Notion?

- **Version control** → every change is tracked.  
- **Portability** → clone anywhere, no vendor lock-in.  
- **Markdown** → simple, universally supported.  
- **Extensible** → later you can add scripts to search, generate PDFs, or build a static site.

---

## 📖 Credits

Original structure adapted from the [tldr-pages project](https://github.com/tldr-pages/tldr).  
All additions and curation beyond that are my own.
