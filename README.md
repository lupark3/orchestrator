# 🚀 Gemini CLI Commands Toolkit

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Environment](https://img.shields.io/badge/OS-Linux-orange.svg)]()
[![Validated](https://img.shields.io/badge/CLI-Gemini_CLI-blue.svg)]()

Um conjunto de comandos avançados para o [Gemini CLI](https://github.com/google/gemini-cli), projetados para transformar a IA em um agente de pesquisa profunda e execução autônoma.

## 🛠️ Especificações de Ambiente
- **Testado em:** Linux (Ubuntu/Debian recomendado).
- **Formato:** Arquivos `.toml` nativos para o diretório `.gemini/commands/`.
- **Portabilidade:** Para uso em outras ferramentas CLI, é necessário converter os prompts para `.md` ou `YAML`, respeitando as tags de instruções.

---

## 🧠 Como os Comandos Funcionam

Este toolkit opera sob o princípio da **Auditabilidade Total**. Cada comando cria um **Caderno de Campo** (.md) na raiz do projeto, registrando cada ação, busca e decisão tomada.

### 1. `/deep-research` (O Investigador)
Focado em extrair a verdade através de múltiplas fontes.
- **Fluxo:** Mapeia sub-perguntas ➔ Executa buscas (mín. 50) ➔ Lê conteúdo real via `web_fetch` (mín. 20 fontes) ➔ Realiza validação cruzada.
- **Saída:** Um relatório técnico consolidado baseado apenas em dados reais (sem alucinações).
- **Consumo:** Elevado (High Token Usage). Use para pesquisas críticas.

### 2. `/manus` (O Orquestrador)
Projetado para planejar e executar tarefas complexas de sistema.
- **Fluxo:** Analisa a tarefa ➔ Detecta cadernos de pesquisa anteriores (Handoff) ➔ Cria um Plano de Execução ➔ Executa comandos shell ➔ Realiza self-debugging automático.
- **Workflow Integrado:** Pode ler o caderno gerado pelo `/deep-research` para aplicar recomendações técnicas precisas.

---

## 🛡️ Fluxo de Aprovação (Gates)

Ambos os comandos utilizam o sistema de **⚠️ GATES** para garantir segurança e controle do usuário. O agente **PARA COMPLETAMENTE** a execução e aguarda sua resposta nos seguintes momentos:

- **G0/G1:** Refinamento de prompt e desambiguação inicial.
- **G2:** Detecção de cadernos anteriores (Retomar vs. Novo).
- **G3:** Aprovação do Plano de Execução (no caso do Manus).
- **G4/G5:** Antes de comandos críticos ou destrutivos (ex: `rm`, `config`).
- **G8:** Finalização e Cleanup (Decisão de manter ou apagar cadernos de log).

---

## 🆙 Evolução: De Commands para Skills

Embora o repositório forneça comandos prontos, você pode transformá-los em **Skills** para maior modularidade:
1. **Pasta:** Crie `.gemini/skills/nome-da-skill/`.
2. **Arquivo:** Adicione um `SKILL.md`.
3. **Migração:** Transfira a `<description>` e as `<instructions>` do arquivo `.toml` para o Markdown.
4. **Contexto:** Isso permite que a IA carregue essas instruções sob demanda, economizando tokens em conversas genéricas.

---

## ⚠️ Avisos Críticos
- **Consumo de Tokens:** Por serem comandos de "Pesquisa Profunda" e "Orquestração", eles utilizam ferramentas de leitura em massa. Utilize em situações complexas onde a precisão é mandatória.
- **Higiene de Dados:** Os cadernos de campo registram tudo. Certifique-se de não expor o conteúdo desses arquivos se eles contiverem dados sensíveis processados.

---
**Desenvolvido para máxima precisão e autonomia no terminal.**
