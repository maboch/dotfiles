### Communication
- Communicate with me in Polish in chat window, but use English in code
- Use English where necessary
- When you need any documentation, use context7 mcp first
- If @docs or @web exists in the prompt, be sure to consider them when answering/implementing the prompt
- Use English in remarks and comments
- Write LLM prompts in English. Make them respond in English unless otherwise specified with {{language}}
- Use concise responses, especially in summaries. Bullet-only. No essays. 2-5 bullets max.
❌ verbose:
```
Zaimplementowałem nową funkcjonalność, która pozwala użytkownikowi wyszukiwać produkty po nazwach. Klient wnioskował o nią. Funkcjonalność używa indeksów bazy danych do poprawy wydajności.
```
✅ concise:
```
funkcjonalność: dodane wyszukiwanie produktów po nazwie
- używa indeksów db dla wydajności
- dodane na prośbę klienta
```

### Code Structure & Modularity
- **Never create a file longer than 500 lines of code.** If a file approaches this limit, refactor by splitting it into modules or helper files.
- **Organize code into clearly separated modules**, grouped by feature or responsibility.
- **Use clear, consistent imports** (prefer relative imports within packages).

### Documentation & Explainability
- **Update `README.md`** when new features are added, dependencies change, or setup steps are modified.
- **Comment non-obvious code** and ensure everything is understandable to a mid-level developer.
- When writing complex logic, **add an inline `# Reason:` comment** explaining the why, not just the what.

### AI Behavior Rules
- **Never assume missing context. Ask questions if uncertain.**
- **Never hallucinate libraries or functions** – only use known, verified packages/libraries.
- **Always confirm file paths and module names** exist before referencing them in code or tests.
- **Never delete or overwrite existing code** unless explicitly instructed to.

### Agents
- **Never switch automatically from Plan to Code mode unless explicitly instructed to do so.**
