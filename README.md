# 🏦 Êxodo Bot — Agente Financeiro Educacional com IA Generativa

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)](https://streamlit.io/)
[![Ollama](https://img.shields.io/badge/Ollama-Local_LLM-000000?style=for-the-badge&logo=ollama&logoColor=white)](https://ollama.com/)
[![DIO](https://img.shields.io/badge/DIO-Bootcamp_IA-8B5CF6?style=for-the-badge)](https://dio.me/)
[![License](https://img.shields.io/badge/Licença-MIT-22C55E?style=for-the-badge)](LICENSE)

> **Guiando investidores iniciantes para fora do deserto da ignorância financeira** — 100% local, seguro e sem alucinações.

[🎬 Assista ao Pitch](#-pitch) • [⚡ Como Executar](#-como-executar) • [🏗 Arquitetura](#-arquitetura) • [📚 Base de Conhecimento](#-base-de-conhecimento)

</div>

---

## 📖 Sobre o Projeto

O **Êxodo Bot** é um agente de Inteligência Artificial desenvolvido para o **Desafio de Agentes de IA da DIO** (Digital Innovation One). O nome é uma referência ao Êxodo bíblico — assim como Moisés guiou o povo pelo deserto, o bot guia o investidor iniciante para fora do "deserto da ignorância financeira" em direção à **autonomia no mercado de capitais**.

O diferencial central é a **arquitetura de segurança e compliance**: o agente nunca recomenda ativos, nunca promete rentabilidade e nunca inventa dados — se a informação não estiver na base de conhecimento, ele admite que não sabe.

```
💡 Educação financeira responsável, acessível e 100% local.
```

---

## ✨ Funcionalidades Principais

| Funcionalidade | Descrição |
|---|---|
| 🔒 **Execução 100% Local** | O LLM roda na própria máquina via Ollama — nenhum dado é enviado para APIs externas |
| 🛡️ **Travas Anti-Alucinação** | Regras inegociáveis bloqueiam recomendações, promessas de ganho e tópicos fora do escopo |
| 🎯 **RAG com Roteamento Dinâmico** | Analisa a pergunta e injeta apenas os documentos relevantes, economizando tokens |
| 🌊 **Respostas em Streaming** | Interface fluida com Streamlit Chat que exibe a resposta em tempo real |
| 📊 **Monitoramento de Métricas** | Consumo de tokens e latência monitorados em segundo plano |
| 🧠 **Nivelamento de Perfil** | Antes de responder tópicos avançados, verifica o nível do usuário |

---

## 🏗 Arquitetura

```
┌─────────────────────────────────────────────────────────┐
│                     USUÁRIO (Browser)                    │
└──────────────────────────┬──────────────────────────────┘
                           │ pergunta
                           ▼
┌─────────────────────────────────────────────────────────┐
│              INTERFACE  ·  Streamlit Chat                │
└──────────────────────────┬──────────────────────────────┘
                           │
              ┌────────────▼─────────────┐
              │   ROTEADOR DE CONTEXTO   │  ← selecionar_contexto()
              │  (Keyword Matching RAG)  │
              └────────────┬─────────────┘
                           │ injeta apenas JSONs relevantes
        ┌──────────────────┼──────────────────┐
        ▼                  ▼                  ▼
   CVM_bolsa.json   glossario_b3.json   produtos.json  …
        └──────────────────┬──────────────────┘
                           │ contexto montado
                           ▼
┌─────────────────────────────────────────────────────────┐
│   SYSTEM PROMPT  +  CONTEXTO  +  HISTÓRICO (últimas 10) │
└──────────────────────────┬──────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│         OLLAMA  ·  qwen2.5-coder:14b  (Local)           │
│          temperature=0.3 · top_p=0.9 · stream=True      │
└─────────────────────────────────────────────────────────┘
```

### Stack Tecnológica

| Camada | Tecnologia | Propósito |
|---|---|---|
| Interface (UI) | Streamlit | Chat interativo com suporte a streaming |
| Orquestrador LLM | Ollama | Execução local de grandes modelos de linguagem |
| Modelo Base | `qwen2.5-coder:14b` | Raciocínio lógico e geração de texto em português |
| RAG | Python 3 (keyword matching) | Seleção dinâmica de contexto por palavras-chave |
| Base de Conhecimento | JSON files | Dados estruturados de literatura financeira e CVM |

---

## 📚 Base de Conhecimento

A base é composta por arquivos JSON que representam conteúdo de fontes consagradas e órgãos reguladores:

| Arquivo | Conteúdo | Palavras-chave de Ativação |
|---|---|---|
| `CVM_bolsa.json` | Diretrizes, regras e prevenção a fraudes da CVM | `cvm`, `regulação`, `fraude`, `regra` |
| `glossario_b3_base.json` | Termos e jargões da Bolsa de Valores | `o que é`, `ação`, `fii`, `dividendo`, `etf` |
| `produtos_financeiros.json` | Explicação sobre classes de ativos | `renda fixa`, `tesouro`, `cdb`, `fundo` |
| `json_investidor_inteligente.json` | Filosofia de Benjamin Graham (Value Investing) | `margem de segurança`, `graham`, `valor intrínseco` |
| `json_mil_ao_milhao.json` | Pilares da riqueza segundo Thiago Nigro | `poupar`, `rentabilidade`, `do mil ao milhão` |
| `perfil_investidor.json` | Mapeamento de perfis de risco | `perfil`, `conservador`, `arrojado` |

> **Fallback inteligente:** quando nenhum arquivo é ativado, o agente aciona uma mensagem de alerta ao sistema para aplicar o protocolo de fallback, evitando respostas fabricadas.

---

## 🛡️ Regras de Compliance (Travas Inegociáveis)

O System Prompt contém 5 travas que nunca podem ser violadas:

```
1. CONCISÃO          → Respostas curtas para evitar cortes no streaming
2. FORA DO ESCOPO    → Recusa assuntos como cripto, IR sobre cripto, etc.
3. NIVELAMENTO       → Pergunta o nível do usuário antes de tópicos avançados
4. SEM ANÁLISE       → Nunca avalia tickers específicos (PETR4, VALE3, etc.)
5. ANTI-ALUCINAÇÃO   → Só responde com dados presentes na base de conhecimento
```

---

## ⚡ Como Executar

### Pré-requisitos

- **Python 3.10+**
- **Ollama** instalado — [download aqui](https://ollama.com/download)

### Passo a passo

**1. Clone o repositório**
```bash
git clone https://github.com/MateusUchoa/Desafio_Dio_Agentes_IA.git
cd Desafio_Dio_Agentes_IA
```

**2. Baixe o modelo LLM** *(pode demorar alguns minutos no primeiro download)*
```bash
ollama pull qwen2.5-coder:14b
```

**3. Instale as dependências Python**
```bash
pip install streamlit ollama
```

**4. Execute a aplicação**
```bash
streamlit run src/Êxodo_bot.py
```

**5. Acesse no navegador**
```
http://localhost:8501
```

---

## 📁 Estrutura do Repositório

```
📦 Desafio_Dio_Agentes_IA/
│
├── 📄 README.md                          # Documentação principal
│
├── 📂 src/
│   └── 🐍 Êxodo_bot.py                  # Código-fonte principal (Streamlit + Ollama + RAG)
│
├── 📂 data/                              # Base de Conhecimento RAG
│   ├── CVM_bolsa.json
│   ├── glossario_b3_base.json
│   ├── produtos_financeiros.json
│   ├── perfil_investidor.json
│   ├── json_investidor_inteligente.json
│   └── json_mil_ao_milhao.json
│
├── 📂 docs/                              # Documentação do desafio DIO
│   ├── 01-documentacao-agente.md
│   ├── 02-base-conhecimento.md
│   ├── 03-prompts.md
│   ├── 04-metricas.md
│   └── 05-pitch.md
│
└── 📂 assets/                            # Imagens e recursos visuais
```

---

## 📊 Avaliação e Métricas

Quatro cenários foram testados para validar o comportamento do agente:

| # | Cenário | Resultado |
|---|---|---|
| 1 | Conceito da base (Margem de Segurança — Graham) | ✅ Correto |
| 2 | Recusa análise de ticker específico (PETR4) | ✅ Correto |
| 3 | Fallback para assunto fora do escopo (Cripto) | ⚠️ A melhorar |
| 4 | Nivelamento antes de explicar derivativos | ⚠️ A melhorar |

**O que funcionou bem:**
- Recusa exemplar de recomendação de ativo (teste 2)
- Assertividade conceitual com uso correto da base de conhecimento (teste 1)

**Próximas melhorias:**
- Reforçar no prompt o protocolo de fallback para tópicos fora do escopo
- Adicionar instrução explícita de nivelamento antes de tópicos avançados
- Ajustar `num_predict` para evitar respostas cortadas no streaming

---

## 🎬 Pitch

📹 **Vídeo de apresentação:** [Assistir no Google Drive](https://drive.google.com/file/d/16Ipudvk1LjAqDJl5RrpGanhxV-xbu_zU/view?usp=drivesdk)

---

## 👨‍💻 Autor

<table>
  <tr>
    <td align="center">
      <a href="https://github.com/MateusUchoa">
        <img src="https://avatars.githubusercontent.com/u/293749902?v=4" width="80px" alt="Mateus Uchoa"/><br/>
        <b>Mateus Uchoa</b>
      </a><br/>
      <sub>Desenvolvedor do Êxodo Bot</sub>
    </td>
  </tr>
</table>

---

## 🏆 Desafio

Este projeto foi desenvolvido como entrega do **Desafio de Agentes de IA** promovido pela [DIO — Digital Innovation One](https://dio.me/), dentro do bootcamp de Inteligência Artificial.

---

<div align="center">

**⭐ Se esse projeto te ajudou, deixe uma estrela no repositório!**

[![GitHub Stars](https://img.shields.io/github/stars/MateusUchoa/Desafio_Dio_Agentes_IA?style=social)](https://github.com/MateusUchoa/Desafio_Dio_Agentes_IA/stargazers)
