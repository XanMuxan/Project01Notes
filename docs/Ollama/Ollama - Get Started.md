# Quickstart
[[Ollama]] доступна на macOS, Windows и Linux. [Загрузить Ollama](https://ollama.com/download)

# Get Started
Запустите ollama в своём терминале чтобы открыть интерактивное меню:
```
ollama
```
 Используйте верхнею и нижнею стрелки для навигации, нажмите enter для запуска, используйте правую стрелку чтобы поменять модель и esc для выхода.

Меню предоставляет быстрый доступ к:

- **Run a model** - Начать интерактивный чат
- **Launch tools** - Claude Code, Codex, OpenClaw, и другое
- **Additional integrations** - Доступно в “More…”

# Помощь
Запустите [OpenClaw](https://docs.ollama.com/integrations/openclaw), персональный AI со 100+ возможностей:

```
ollama launch openclaw
```

# Coding
Запустите [Claude Code](https://docs.ollama.com/integrations/claude-code) and other coding tools with Ollama models:

```
ollama launch claude
```

```
ollama launch codex
```

```
ollama launch opencode
```

# API
Используйте [API](https://docs.ollama.com/api) для интеграции [[Ollama]] в ваши приложения:

```
curl http://localhost:11434/api/chat -d '{
  "model": "gemma3",
  "messages": [{ "role": "user", "content": "Hello!" }]
}'
```

#LM #GetStarted #Ollama