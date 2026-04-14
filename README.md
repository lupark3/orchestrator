# 🚀 Gemini CLI Orchestrator Toolkit

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Environment](https://img.shields.io/badge/OS-Linux-orange.svg)]()
[![Validated](https://img.shields.io/badge/CLI-Gemini_CLI-blue.svg)]()

Um conjunto avançado de **Commands** e **Agents** para o [Gemini CLI](https://github.com/google-gemini/gemini-cli), projetado para transformar a IA em um sistema de pesquisa profunda e execução autônoma. 

> **Aviso de Compatibilidade:** Este toolkit foi exaustivamente **testado e validado exclusivamente no Gemini CLI**. Para uso em outras plataformas ou ferramentas CLI de IA, adaptações estruturais nos prompts e comandos podem ser necessárias.

---

## 📂 Estrutura do Toolkit e Deep Links

Este repositório fornece as ferramentas em dois formatos. Você pode instalar ambos diretamente usando os deep links:

### 1. Commands (`/commands`)
Arquivos `.toml` nativos do Gemini CLI. Fornecem instruções estritas que você chama sob demanda no chat (ex: `/deep-research`, `/manus`, `/deep-research-lite`).

**Instalação via Deep Link (Comandos):**
```bash
gemini commands install https://raw.githubusercontent.com/SEU_USUARIO/orchestrator/main/commands/deep-research.toml
gemini commands install https://raw.githubusercontent.com/SEU_USUARIO/orchestrator/main/commands/deep-research-lite.toml
gemini commands install https://raw.githubusercontent.com/SEU_USUARIO/orchestrator/main/commands/manus.toml
```

### 2. Agents (`/agents`)
Arquivos `.md` que atuam como sub-agentes especialistas no ecossistema do Gemini CLI, com diretrizes otimizadas para carregamento sob demanda.

**Vantagem principal:** Por rodarem como sub-agentes, eles executam tarefas complexas e extensas em um fluxo de contexto isolado, preservando totalmente a sua sessão principal (janela de contexto). O chat principal se mantém limpo e sem estouro de tokens, pois apenas o resultado final (e não a longa execução) é retornado para a conversa base.

**Instalação via Deep Link (Agentes):**
```bash
gemini agents add https://raw.githubusercontent.com/SEU_USUARIO/orchestrator/main/agents/deep-research.md
gemini agents add https://raw.githubusercontent.com/SEU_USUARIO/orchestrator/main/agents/deep-research-lite.md
gemini agents add https://raw.githubusercontent.com/SEU_USUARIO/orchestrator/main/agents/manus.md
```

*(Nota: Substitua `SEU_USUARIO` pelo seu usuário do GitHub caso faça fork, ou use a URL direta do repositório original).*

---

## 🧠 Ferramentas Disponíveis

Todas as abordagens operam sob o princípio da **Auditabilidade Total** e **Alta Interatividade**. Cada execução cria um **Caderno de Campo** (.md) na raiz do projeto, registrando cada ação, busca e decisão. Além disso, **os agentes sempre solicitarão autorização explícita do usuário** antes de realizar qualquer alteração no sistema ou executar ações críticas.

### 1. `deep-research-lite` (Pesquisa Ágil)
Versão 50% mais rápida da pesquisa profunda. Metas reduzidas para maior agilidade, mantendo a obrigatoriedade do caderno de campo.
- **Uso:** Ideal para investigações detalhadas com menor volume de fontes.

### 2. `deep-research` (O Investigador)
Focado em extrair a verdade através de múltiplas fontes.
- **Fluxo:** Mapeia sub-perguntas ➔ Buscas (mín. 50) ➔ Leitura via `web_fetch` (mín. 20 fontes) ➔ Validação cruzada.
- **Saída:** Relatório técnico consolidado sem alucinações.
- **Consumo:** Elevado. Use para pesquisas críticas.

### 3. `manus` (O Orquestrador)
Projetado para planejar e executar tarefas complexas de sistema de forma autônoma, mas sempre sob supervisão.
- **Fluxo:** Analisa tarefa ➔ Handoff (lê caderno do deep-research) ➔ Cria Plano ➔ Solicita Aprovação ➔ Executa shell ➔ Self-debugging.
- **Integração:** Lê nativamente os relatórios gerados pelas ferramentas de pesquisa para guiar suas ações.

---

## 🔗 Sobre os Aliases de Modelo e Configuração (IMPORTANTE)

Nos arquivos dos agentes (`/agents`), você notará a configuração de **aliases de modelo** na definição do agente (exemplo: `model = "pro-research"`). 
Esses aliases referem-se à **configuração de modelos do próprio Gemini CLI**. O `pro-research`, por exemplo, é um alias que mapeia para o modelo Gemini Pro com as capacidades de **pensamento profundo (thinking)** ativadas.

**⚠️ ALERTA DE QUEBRA:** O Gemini CLI **vai quebrar (crash)** ao tentar rodar um agente que referencia um alias que não existe na sua máquina.

### Como configurar os Aliases no seu ambiente
Para que os agentes rodem com força total e ativem o "pensamento profundo" de maneira correta, você precisa adicionar a configuração do alias no seu arquivo `~/.gemini/settings.json`. Abra esse arquivo e adicione o bloco `modelConfigs`:

```json
{
  "modelConfigs": {
    "customAliases": {
      "pro-research": {
        "modelConfig": {
          "model": "gemini-3.1-pro-preview",
          "generateContentConfig": {
            "thinkingConfig": {
              "thinkingLevel": "HIGH"
            }
          }
        }
      },
      "pro-research-lite": {
        "modelConfig": {
          "model": "gemini-3.1-pro-preview",
          "generateContentConfig": {
            "thinkingConfig": {
              "thinkingLevel": "MEDIUM"
            }
          }
        }
      }
    }
  }
}
```

### Alternativa: Como remover a dependência
Caso você **não tenha** ou **não queira** configurar esses aliases específicos no seu Gemini CLI, você deve remover a obrigatoriedade dos agentes instalados.

Basta abrir o arquivo `.md` do agente (ex: `~/.gemini/agents/manus.md`) e **remover a linha `model = ...`** ou alterá-la para o seu modelo de preferência. Sem essa linha, ele usará o modelo padrão global do seu ambiente e não vai quebrar.

---

## 🛡️ Fluxo de Aprovação (Gates) e Interatividade

Estas ferramentas são **altamente interativas**. O agente **NUNCA** age pelas suas costas em ações de risco. Ele utiliza **⚠️ GATES** (pontos de parada) para garantir segurança e controle absoluto do usuário:
- **G0/G1:** Refinamento de prompt e desambiguação.
- **G2:** Detecção de cadernos de pesquisa/execução anteriores (Decisão de Handoff).
- **G3:** Aprovação do Plano de Execução (Obrigatório no Manus).
- **G4/G5:** Autorização obrigatória antes de executar comandos de shell críticos ou destrutivos.
- **G8:** Finalização e Cleanup (Manter, arquivar ou apagar cadernos de log).

---

## ⚠️ Avisos Críticos
- **Consumo de Tokens:** As pesquisas e orquestrações realizam leituras em massa na web e no sistema.
- **Higiene de Dados:** Os cadernos de campo registram absolutamente tudo. Certifique-se de não commitar/expor cadernos que contenham chaves ou dados sensíveis processados.

---
**Desenvolvido para máxima precisão e autonomia segura no terminal.**
