# GitHub Copilot Instructions

Estas instruções ajudam o Copilot e outros agentes de codificação a manter a consistência neste repositório.

## 🐍 Python Style
- Use **Ruff** para linting e formatação.
- Siga o **PEP 8**.
- Prefira `Typer` para interfaces CLI.
- Docstrings devem seguir o formato Google.

## 🐚 Bash Style
- Inicie sempre com `#!/usr/bin/env bash`.
- Use `set -euo pipefail` para segurança.
- Valide scripts com `shellcheck`.
- Utilize `[[ ]]` para testes e aspas em todas as variáveis.

## 📖 Markdown & Docs
- Use **Markdownlint** para formatação.
- Mantenha os cadernos de campo atualizados conforme a Trava Mecânica descrita em `AGENTS.md`.
- Diagramas devem usar **Mermaid.js**.

## 🤖 AI Interaction
- Ao sugerir mudanças, priorize soluções que minimizem o consumo de tokens.
- Documente decisões técnicas importantes em arquivos `.md` na pasta `/docs`.
