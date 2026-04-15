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
Arquivos `.toml` nativos do Gemini CLI. Fornecem instruções estritas que você chama sob demanda no chat (ex: `/pesquisa`, `/manus`). Eles orquestram o fluxo de execução chamando os agentes adequados no momento certo.

**Instalação via Deep Link (Comandos):**
```bash
gemini commands install https://raw.githubusercontent.com/SEU_USUARIO/orchestrator/main/commands/pesquisa.toml
gemini commands install https://raw.githubusercontent.com/SEU_USUARIO/orchestrator/main/commands/manus.toml
```
*(Nota: Os antigos `deep-research.toml` e `deep-research-lite.toml` foram consolidados no comando unificado `/pesquisa` para evitar pulo de etapas e melhorar a confiabilidade).*

### 2. Agents (`/agents`)
Arquivos `.md` que atuam como sub-agentes especialistas no ecossistema do Gemini CLI, com diretrizes otimizadas para carregamento sob demanda.

**Vantagem principal:** Por rodarem como sub-agentes, eles executam tarefas complexas e extensas em um fluxo de contexto isolado, preservando totalmente a sua sessão principal (janela de contexto). O chat principal se mantém limpo e sem estouro de tokens, pois apenas o resultado final (e não a longa execução) é retornado para a conversa base.

Para garantir que o fluxo de pesquisa não sofra com quebras de raciocínio ou pule etapas obrigatórias, o fluxo principal foi dividido em 4 sub-agentes especialistas orquestrados pelo comando `/pesquisa`.

**Instalação via Deep Link (Agentes):**
```bash
gemini agents add https://raw.githubusercontent.com/SEU_USUARIO/orchestrator/main/agents/orq-criador.md
gemini agents add https://raw.githubusercontent.com/SEU_USUARIO/orchestrator/main/agents/orq-pesquisador.md
gemini agents add https://raw.githubusercontent.com/SEU_USUARIO/orchestrator/main/agents/orq-extrator.md
gemini agents add https://raw.githubusercontent.com/SEU_USUARIO/orchestrator/main/agents/orq-consolidador.md
gemini agents add https://raw.githubusercontent.com/SEU_USUARIO/orchestrator/main/agents/manus.md
```

*(Nota: Substitua `SEU_USUARIO` pelo seu usuário do GitHub caso faça fork, ou use a URL direta do repositório original).*

---

## 🧠 Ferramentas Disponíveis

Todas as abordagens operam sob o princípio da **Auditabilidade Total** e **Alta Interatividade**. Cada execução cria um **Caderno de Campo** (.md) na raiz do projeto, registrando cada ação, busca e decisão. Além disso, **os agentes sempre solicitarão autorização explícita do usuário** antes de realizar qualquer alteração no sistema ou executar ações críticas.

### 1. `/pesquisa` (Orquestrador de Pesquisa Delegada)
Devido à complexidade de gerenciar múltiplas etapas de pesquisa sem pular processos, a pesquisa profunda agora é orquestrada pelo comando `/pesquisa`. Ele coordena 4 sub-agentes especializados para garantir que cada fase seja executada de forma sequencial, confiável e rigorosa. O usuário pode escolher entre os modos **Normal** (~20 buscas) ou **Lite** (~10 buscas).
- **Fluxo de Sub-agentes:**
  1. **`orq-criador`:** Prepara o ambiente e cria o caderno de pesquisa isolado.
  2. **`orq-pesquisador`:** Realiza buscas iterativas no Google e anota URLs relevantes.
  3. **`orq-extrator`:** Acessa as URLs em lote via `web_fetch` (com fallback em Playwright para páginas bloqueadas) e salva o conteúdo integral.
  4. **`orq-consolidador`:** Lê todos os arquivos fonte extraídos e cruza os dados para gerar o `relatorio_final.md`.
- **Saída:** Relatório técnico consolidado, isento de alucinações e acompanhado das fontes brutas salvas em disco.

### 2. `/manus` (O Orquestrador de Execução)
Projetado para planejar e executar tarefas complexas de sistema de forma autônoma, mas sempre sob supervisão.
- **Fluxo:** Analisa a tarefa ➔ Handoff (lê o caderno de pesquisa/relatório) ➔ Cria Plano ➔ Solicita Aprovação ➔ Executa shell ➔ Self-debugging.
- **Integração:** Integrado nativamente ao fluxo do `/pesquisa`. Quando uma pesquisa é finalizada, o sistema oferece o "Handoff", transferindo o contexto levantado para o **Manus** criar e executar uma estratégia baseada na verdade estabelecida pelo relatório final.

---

## 🔗 Sobre os Aliases de Modelo e Configuração (IMPORTANTE)

Nos arquivos dos agentes (`/agents`), você notará a configuração de **aliases de modelo** na definição do agente (exemplo: `model = "pro-research"`). 
Esses aliases referem-se à **configuração de modelos do próprio Gemini CLI**. O `pro-research`, por exemplo, é um alias que mapeia para o modelo Gemini Pro com as capacidades de **pensamento profundo (thinking)** ativadas.

**⚠️ ALERTA DE QUEBRA:** O Gemini CLI **vai quebrar (crash)** ao tentar rodar um agente que referencia um alias que não existe na sua máquina.

### Configurações Experimentais (Obrigatórias)
Para que os agentes funcionem corretamente e consigam realizar web fetch com o conteúdo integral das páginas, você **deve** ativar as flags experimentais correspondentes.

Abra o seu arquivo `~/.gemini/settings.json` e certifique-se de que o bloco `experimental` esteja configurado da seguinte forma:

```json
{
  "experimental": {
    "directWebFetch": true,
    "enableAgents": true
  }
}
```
- **`directWebFetch: true`**: Permite que o sub-agente `orq-extrator` traga o conteúdo textual bruto e integral das páginas da web, essencial para a análise profunda, contornando resumos limitados de buscas padrão.
- **`enableAgents: true`**: Habilita o suporte nativo do Gemini CLI à execução de sub-agentes, que é a base arquitetural de todo este toolkit.

### Como configurar os Aliases no seu ambiente
Para que os agentes rodem com força total e ativem o "pensamento profundo" de maneira correta, você precisa adicionar a configuração do alias no seu arquivo `~/.gemini/settings.json`. Adicione o bloco `modelConfigs`:

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
- **G0/G1:** Refinamento de prompt e desambiguação (Oferecido via `/pesquisa` nos Modos).
- **G2:** Detecção de cadernos de pesquisa e Handoff (Decisão de transição para o Manus).
- **G3:** Aprovação do Plano de Execução (Obrigatório no Manus).
- **G4/G5:** Autorização obrigatória antes de executar comandos de shell críticos ou destrutivos.
- **G8:** Finalização e Cleanup (Manter, arquivar ou apagar cadernos de log).

---

## ⚠️ Avisos Críticos
- **Consumo de Tokens:** As pesquisas e orquestrações realizam leituras em massa na web e no sistema. O uso de sub-agentes mitiga o impacto na sua janela principal de chat, mas atente-se às limitações da API.
- **Higiene de Dados:** Os cadernos de campo e arquivos fonte registram absolutamente tudo o que for pesquisado e extraído. Certifique-se de não commitar/expor cadernos que contenham dados sensíveis processados.

---
**Desenvolvido para máxima precisão e autonomia segura no terminal.**
