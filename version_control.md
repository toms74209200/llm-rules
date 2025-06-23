## Version Control and Commits

- Always use [gitmoji](https://gitmoji.dev/) in commit messages
- When possible, access the gitmoji MCP server to get correct gitmoji, as LLM's remembered gitmoji may be hallucinated
   - ✨ (`:sparkles:`) - New features
   - 🐛 (`:bug:`) - Bug fixes
   - ♻️ (`:recycle:`) - Code refactoring
   - 📝 (`:memo:`) - Documentation updates
   - ✅ (`:white_check_mark:`) - Add or update tests
   - 🎨 (`:art:`) - Improve code structure/format
   - 🔨 (`:hammer:`) - Add or update development scripts
   - 🔧 (`:wrench:`) - Add or update configuration files
- Commit format: `:emoji: concise description in English`
- Use `git switch -c <branch_name>` to create branches
- Make separate commits for different types/files/features of changes
- IMPORTANT: Always check changes with `git status` before committing
