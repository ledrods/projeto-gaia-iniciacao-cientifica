# Descrição de Projeto: Sistema de Validação de Conteúdo com GenAI (IC)

**Nota Importante:** *Este projeto foi desenvolvido como parte de uma Iniciação Científica (IC) em um ambiente com código-fonte confidencial. Este `README` descreve a arquitetura, os desafios e as minhas contribuições em alto nível, sem expor qualquer código ou dado proprietário.*

---

### 1. Contexto e Problema de Negócio

O objetivo deste projeto de pesquisa era automatizar a criação de questões educacionais (estilo SAEB) alinhadas à Base Nacional Comum Curricular (BNCC), utilizando IA Generativa.

O desafio central não era apenas *gerar* questões, mas garantir sua **qualidade pedagógica** em escala. Questões geradas por IA podem ser fáceis/difíceis demais ou não avaliar a habilidade (competência) correta.

Minha atuação foi central em um **Sistema de Agentes de Validação**, responsável por analisar e garantir a qualidade das questões geradas antes de serem usadas.

---

### 2. Minha Contribuição Principal: Módulos de Validação

Eu fui responsável por projetar e implementar dois componentes críticos deste sistema:

#### Contribuição A: Módulo de Validação de Dificuldade (Simulador de Alunos)

* **O Problema:** Como saber se uma questão gerada é "fácil", "média" ou "difícil" de forma objetiva, sem depender de testes manuais demorados com alunos reais?
* **Minha Solução:** Desenvolvi um **Agente Simulador de Alunos** usando LLMs.
* **Como Funciona:**
    1.  **Engenharia de Prompt:** Criei *prompts* detalhados que instruíam a LLM a agir como um aluno com um nível de habilidade específico (baseado em perfis definidos pela Teoria da Resposta ao Item - TRI).
    2.  **Orquestração:** O sistema "passava" a questão gerada para uma "multidão" de agentes simuladores (com diferentes níveis de habilidade).
    3.  **Análise:** Ao analisar a porcentagem de acerto desses alunos simulados, conseguíamos classificar estatisticamente a dificuldade da questão.
* **Impacto:** Criamos um método escalável e automatizado para validar o parâmetro de dificuldade das questões, permitindo que o agente gerador se autoajustasse.

#### Contribuição B: Módulo de Validação de Habilidades (BNCC)

* **O Problema:** Como garantir que uma questão sobre "Identificar o tema de um texto" (Habilidade-Alvo) não estava, na verdade, avaliando apenas "Localizar informação explícita" (uma habilidade mais simples e "vizinha")?
* **Minha Solução:** Implementei um **Validador de Habilidades** em duas fases, baseado em similaridade de *embeddings*.
* **Como Funciona:**
    1.  **Engenharia de Embeddings:** Para criar uma "tabela de proximidade", gerei *embeddings* para todas as habilidades da BNCC. Fiz isso extraindo palavras-chave de cada habilidade e calculando o cosseno de similaridade entre elas para construir uma matriz de proximidade.
    2.  **Fase 1 (Triagem):** O agente usava essa matriz de similaridade para identificar automaticamente quais habilidades eram mais "próximas" da habilidade-alvo da questão.
    3.  **Fase 2 (Validação por IA):** Com essa lista curta, o agente usava LLMs e Engenharia de Prompt para analisar a questão e fornecer um feedback detalhado (ex: "Habilidade-alvo: 80% de cobertura", "Habilidade-secundária-1: 50% de cobertura").
* **Impacto:** O agente de geração recebia um feedback rico e automático para se corrigir. Se uma questão estava fraca na habilidade-alvo, ela era alterada ou descartada, garantindo a **qualidade e precisão pedagógica** do conteúdo.

---

### 3. Stack Técnica & Conceitos Aplicados

* **Linguagens & Ferramentas:** Python, Git
* **Conceitos de GenAI:** Engenharia de Prompt Avançada, Orquestração de Agentes (Multi-Agentes).
* **LLMs (Agnóstico):** Orquestração e testes com múltiplos modelos, incluindo **DeepSeek**, **OpenAI (gpt-oss-20b)**, **Llama 3.3 70B (via Groq)** e **Gemini 1.5 Flash**.
* **ML/NLP:** Geração de Embeddings (Sentence Transformers), Cálculo de Similaridade de Cosseno.
* **Conceitos de Produto:** Ciclos de Experimentação, Validação de Métricas (Factualidade, *Groundedness*), Colaboração com Especialistas de Domínio (Pedagogos).
