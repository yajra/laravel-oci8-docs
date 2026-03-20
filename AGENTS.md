# AGENTS.md

## Agent Guidelines for Documentation

When working with this documentation repository, ALWAYS follow these rules:

### Source of Truth
- **Read the `src` folder first** - This folder contains the actual code being documented
- Never document methods that don't exist in the source code
- Verify method signatures by grepping the codebase before documenting

### Heading Anchors
**ALWAYS add `<a name="anchor-id"></a>` anchors before each `##` heading.**

- Anchors are required for all H2 sections
- Example: `<a name="usage"></a>` before `## Usage`
- Verify anchors exist after every edit - they get repeatedly removed

### Internal Links
**Never use `.md` extension in internal links.**

- Correct: `[Link](filename)`
- Wrong: `[Link](filename.md)`
- External URLs (e.g., Packagist, GitHub) may keep their full URL

### API Reference Links
- Do NOT hallucinate API reference pages
- If no API reference page exists, remove the link rather than creating dead links

### Code Examples
- Keep examples simple and developer-friendly
- Use the latest PHP syntax (PHP 8.3+)
- Use `App\Models\User` not `App\User`
- Ensure examples are copy-paste friendly

### Version Constraints
- Use `^13` for Laravel-OCI8 version constraints
- The package follows Laravel's major version: Laravel 13 → Laravel-OCI8 13

### File Consistency
- When merging content, remove the old file and update `documentation.md`
- When adding content, add anchors—don't remove existing ones
