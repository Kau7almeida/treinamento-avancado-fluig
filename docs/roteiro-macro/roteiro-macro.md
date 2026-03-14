# 🎯 ROTEIRO MACRO — Treinamento Avançado Fluig (48h)
## JYNX | Novo Cliente | Início: 17/03/2026

---

## METADADOS DO TREINAMENTO

| Item | Detalhe |
|------|---------|
| **Formato** | 12 sessões × 4h (segunda e quinta) |
| **Duração total** | 48 horas |
| **Período** | 17/03/2026 a 04/05/2026 (7 semanas) |
| **Perfil da turma** | Mix de desenvolvedores e analistas |
| **Projeto Integrador** | Sistema de Solicitação de Compras (evolui da sessão 05 à 12) |
| **Filosofia** | 70% hands-on / 30% conceitual · Raciocínio arquitetural (o "porquê") |
| **Escopo de Integração** | Apenas GET (consultas) via REST ao Protheus · Datasets criados do zero |
| **Consultas Protheus** | SA2 (Fornecedores), SB1 (Produtos), CTT (Centros de Custo), SE4 (Cond. Pagamento) |
| **Autenticação Protheus** | Basic Auth padrão · Verificar credenciais com cliente antes da sessão 10 |

---

## CALENDÁRIO GERAL

| # | Data | Dia | Tema | Bloco Temático |
|---|------|-----|------|----------------|
| 01 | 17/03 (seg) | Seg | Introdução, Ferramentas e Boas Práticas | 🟦 FUNDAÇÃO |
| 02 | 20/03 (qui) | Qui | Configuração do Ambiente e fluig Style Guide | 🟦 FUNDAÇÃO |
| 03 | 24/03 (seg) | Seg | Formulários Customizados e Dinâmicos — Fundamentos | 🟩 FORMULÁRIOS |
| 04 | 27/03 (qui) | Qui | Formulários Avançados e Técnicas Dinâmicas | 🟩 FORMULÁRIOS |
| 05 | 31/03 (seg) | Seg | Projeto Integrador I — Início do Projeto Fluig | 🟨 PROJETO |
| 06 | 03/04 (qui) | Qui | Workflows e BPMN 2.0 — Fundamentos | 🟪 WORKFLOWS |
| 07 | 07/04 (seg) | Seg | Workflows Avançados e Eventos de Formulário | 🟪 WORKFLOWS |
| 08 | 10/04 (qui) | Qui | Projeto Integrador II — Evolução do Projeto | 🟨 PROJETO |
| 09 | 14/04 (seg) | Seg | Datasets Customizados e Introdução a Integrações REST | 🟧 DADOS |
| 10 | 17/04 (qui) | Qui | Integração REST com TOTVS Protheus (Apenas Consultas GET) | 🟧 DADOS |
| 11 | 24/04 (qui) | Qui | Portais, Pages e Widgets | 🟥 PORTAIS |
| 12 | 28/04 (seg) | Seg | Widgets Avançados e Projeto Final | 🟥 PORTAIS |

> ⚠️ **Nota:** 21/04 (seg) = Tiradentes — sessão 11 pula para quinta 24/04. Verificar se há outros feriados regionais do cliente.

---

## ESTRUTURA PADRÃO DE CADA SESSÃO (4h)

Todas as sessões seguem o mesmo ritmo. Isso cria previsibilidade para os alunos e facilita sua gestão de tempo.

```
[00:00 - 00:15]  RECONEXÃO (15 min)
                  → Recapitulação colaborativa: um aluno resume a aula anterior
                  → Instrutor complementa e conecta ao tema do dia

[00:15 - 01:15]  BLOCO CONCEITUAL + DEMO (60 min)
                  → Slides + live coding narrado
                  → Técnica: "erro proposital" → turma identifica → correção

[01:15 - 01:25]  ☕ INTERVALO (10 min)

[01:25 - 02:55]  BLOCO HANDS-ON (90 min)
                  → Exercício guiado (todos juntos)
                  → Exercício autônomo (turma pratica, instrutor circula)
                  → Desafio extra para devs avançados

[02:55 - 03:05]  ☕ INTERVALO (10 min)

[03:05 - 03:45]  BLOCO DE CONSOLIDAÇÃO (40 min)
                  → Revisão do exercício com a turma
                  → Discussão arquitetural: "por que fizemos assim?"
                  → Cenários de erro e troubleshooting

[03:45 - 04:00]  ENCERRAMENTO (15 min)
                  → Resumo do dia (3 takeaways)
                  → Preview do próximo encontro
                  → Dúvidas finais
```

---

## DETALHAMENTO POR SESSÃO

---

### 📘 SESSÃO 01 — Introdução, Ferramentas e Boas Práticas
**Data:** 17/03/2026 (Segunda) · **Bloco:** 🟦 FUNDAÇÃO

**Objetivo:** Entendimento sólido da arquitetura Fluig, componentes da plataforma e como se relacionam em uma solução corporativa.

#### Conteúdo Programático
- Visão geral da plataforma Fluig para desenvolvedores
- Os 5 pilares: Formulários, Processos, Datasets, Portais/Pages, Widgets
- Arquitetura de desenvolvimento (servidor JBoss, engine Rhino JS, banco de dados)
- Conceito de "cards" do Fluig e como o ecossistema se conecta
- Boas práticas de organização de projetos (nomenclatura, versionamento, estrutura de pastas)
- Materiais de estudo e links úteis de apoio (documentação oficial, TDN, Style Guide)

#### 🎯 Estratégia do Instrutor
- **Abertura forte:** Apresentar uma solução Fluig completa funcionando (o Projeto Integrador finalizado) como "trailer" do que vão construir. Isso gera motivação.
- **Mapa mental ao vivo:** Desenhar no quadro/tela os 5 pilares e como se conectam. Pedir aos alunos para dar exemplos do dia a dia deles que encaixam em cada pilar.
- **Discussão arquitetural (tempo):** "Por que o Fluig usa Rhino JS e não Node? Qual a implicação disso?" — essa discussão rende 15-20 min e ensina limitações reais da plataforma.
- **Ganho de tempo:** A parte de "materiais de estudo" pode ser expandida ou comprimida conforme necessidade. Bom buffer.

#### ⏱️ Timing Detalhado
| Tempo | Atividade | Duração |
|-------|-----------|---------|
| 00:00 | Apresentações (instrutor + turma) + expectativas | 20 min |
| 00:20 | "Trailer" do Projeto Integrador finalizado | 10 min |
| 00:30 | Arquitetura da plataforma (slides + mapa mental) | 40 min |
| 01:10 | ☕ Intervalo | 10 min |
| 01:20 | Os 5 pilares em detalhe + demonstração navegando na plataforma | 45 min |
| 02:05 | Boas práticas de projeto (nomenclatura, estrutura) | 25 min |
| 02:30 | ☕ Intervalo | 10 min |
| 02:40 | Exercício: Navegar na plataforma e identificar componentes | 30 min |
| 03:10 | Materiais de estudo + tour pela documentação | 25 min |
| 03:35 | Discussão: "O que vocês esperam automatizar na empresa?" | 15 min |
| 03:50 | Encerramento + preview sessão 02 | 10 min |

#### 🐛 Armadilhas Clássicas & Troubleshooting
- Alunos confundem "formulário Fluig" com formulário HTML puro → Explicar que o form Fluig TEM um HTML por baixo, mas com regras específicas (campos `name` obrigatórios para persistência)
- Confusão entre Fluig Identity e Fluig Plataforma → Esclarecer logo no início

#### 🎁 Desafio Extra (Devs Avançados)
- Pesquisar a documentação e listar 3 classes do SDK que acham mais úteis para o dia a dia

---

### 📘 SESSÃO 02 — Configuração do Ambiente e fluig Style Guide
**Data:** 20/03/2026 (Quinta) · **Bloco:** 🟦 FUNDAÇÃO

**Objetivo:** Ambiente de desenvolvimento pronto e funcionando + domínio dos padrões visuais do Style Guide.

#### Conteúdo Programático
- Configuração do Eclipse + fluig Studio (plugin)
- Estrutura de projetos no fluig Studio (pastas, arquivos de configuração)
- Conexão do Studio com o servidor Fluig
- Introdução ao fluig Style Guide (componentes, grid, responsividade)
- Padrões visuais: botões, inputs, tabelas, modais, alerts
- Primeiro deploy: formulário simples "Hello Fluig"

#### 🎯 Estratégia do Instrutor
- **Sessão técnica por natureza** — aqui problemas de ambiente vão aparecer. Reserve MUITO mais tempo do que acha necessário para troubleshooting.
- **Erro proposital:** Configurar intencionalmente a URL do servidor errada → mostrar o erro → turma diagnostica → corrige junto.
- **Style Guide como galeria:** Abrir o Style Guide online e navegar como se fosse um catálogo. Perguntar: "Se vocês precisassem de um campo de busca, onde procurariam?"
- **Deploy como celebração:** O primeiro deploy funcionando é um marco emocional. Celebre isso com a turma.

#### ⏱️ Timing Detalhado
| Tempo | Atividade | Duração |
|-------|-----------|---------|
| 00:00 | Reconexão: aluno resume sessão 01 | 15 min |
| 00:15 | Instalação/configuração Eclipse + plugin fluig Studio | 45 min |
| 01:00 | Troubleshooting de ambiente (buffer generoso) | 15 min |
| 01:15 | ☕ Intervalo | 10 min |
| 01:25 | Estrutura de projetos no Studio + primeiro projeto | 30 min |
| 01:55 | Conexão com servidor + primeiro deploy ("Hello Fluig") | 35 min |
| 02:30 | ☕ Intervalo | 10 min |
| 02:40 | Tour pelo fluig Style Guide (componentes visuais) | 40 min |
| 03:20 | Exercício: Criar formulário usando 5 componentes do Style Guide | 30 min |
| 03:50 | Encerramento + preview sessão 03 | 10 min |

#### 🐛 Armadilhas Clássicas & Troubleshooting
- Versão do Java incompatível com o Eclipse/plugin → Verificar previamente com TI do cliente
- Plugin do fluig Studio não aparece → Verificar URL de update site
- Erro de conexão com servidor → Geralmente é firewall ou URL incorreta
- Deploy falha silenciosamente → Verificar console do Eclipse para erros

#### 🎁 Desafio Extra (Devs Avançados)
- Criar um formulário com layout responsivo usando grid do Style Guide (2 colunas em desktop, 1 em mobile)

---

### 📗 SESSÃO 03 — Formulários Customizados e Dinâmicos — Fundamentos
**Data:** 24/03/2026 (Segunda) · **Bloco:** 🟩 FORMULÁRIOS

**Objetivo:** Construção de formulários dinâmicos e bem estruturados usando HTML + recursos nativos do Fluig.

#### Conteúdo Programático
- Estrutura de um formulário Fluig (HTML + convenções obrigatórias)
- Inputs de Formulário I: Select, Text, Textarea, Radio, Checkbox, Button
- A tag `<form>` no contexto Fluig: `name` é a chave de persistência
- Helpers do Fluig (funções auxiliares nativas)
- Validações básicas (required, patterns, masks)
- Organização visual com Style Guide (fieldsets, grids, panels)

#### 🎯 Estratégia do Instrutor
- **Comparativo "HTML puro vs Fluig":** Mostrar um form HTML normal e depois o mesmo com as convenções Fluig. Perguntar: "O que mudou? Por quê?"
- **Erro proposital:** Criar um campo sem o atributo `name` → deploy → mostrar que o dado não persiste → explicar que o `name` é o que o Fluig usa para salvar no banco.
- **Arquitetural:** "Por que o Fluig usa HTML direto em vez de um framework próprio de formulários? Vantagens e desvantagens."
- **Live coding narrado:** Construir um formulário do zero, campo a campo, explicando cada decisão.

#### ⏱️ Timing Detalhado
| Tempo | Atividade | Duração |
|-------|-----------|---------|
| 00:00 | Reconexão + aluno resume sessão 02 | 15 min |
| 00:15 | Anatomia de um formulário Fluig (slides + código) | 30 min |
| 00:45 | Live coding: Formulário campo a campo (Text, Select, Radio) | 30 min |
| 01:15 | ☕ Intervalo | 10 min |
| 01:25 | Helpers do Fluig + Validações básicas | 30 min |
| 01:55 | Exercício guiado: Formulário de cadastro com 8+ campos | 40 min |
| 02:35 | ☕ Intervalo | 10 min |
| 02:45 | Exercício autônomo: Formulário personalizado (tema livre) | 35 min |
| 03:20 | Revisão dos exercícios + "por que fizemos assim?" | 25 min |
| 03:45 | Encerramento + preview sessão 04 | 15 min |

#### 🐛 Armadilhas Clássicas & Troubleshooting
- Campo sem `name` → dado não persiste (erro #1 mais comum)
- `name` com caracteres especiais/espaços → comportamento imprevisível
- Esquecer de envolver campos no `<form>` do Fluig → dados perdidos
- CSS customizado conflitando com Style Guide → sempre usar classes do SG primeiro

#### 🎁 Desafio Extra (Devs Avançados)
- Implementar máscara de CPF/CNPJ com jQuery Mask Plugin integrado ao formulário

---

### 📗 SESSÃO 04 — Formulários Avançados e Técnicas Dinâmicas
**Data:** 27/03/2026 (Quinta) · **Bloco:** 🟩 FORMULÁRIOS

**Objetivo:** Formulários avançados com carregamento dinâmico de dados e relacionamento entre campos.

#### Conteúdo Programático
- Inputs de Formulário II: Zoom, AutoComplete, técnica Pai × Filho
- Select populado via Dataset (carregamento dinâmico)
- Listas dinâmicas (adicionar/remover linhas na tabela)
- Boas práticas de usabilidade em formulários complexos
- JavaScript no contexto do formulário Fluig (eventos, manipulação DOM)

#### 🎯 Estratégia do Instrutor
- **Zoom é o "wow moment":** Demonstrar o Zoom funcionando primeiro, depois desconstruir como funciona. Analistas adoram isso.
- **Pai × Filho é complexo:** Ir devagar aqui. Mostrar o fluxo: "Seleciona o Estado → carrega Cidades". Desenhar o diagrama de sequência.
- **Erro proposital:** Montar um Pai × Filho sem limpar o campo filho quando o pai muda → mostrar o bug → corrigir.
- **Lista dinâmica é ouro:** Mostrar como adicionar linhas de itens (tipo itens de uma nota fiscal). Isso é 90% do que a turma vai usar no dia a dia.

#### ⏱️ Timing Detalhado
| Tempo | Atividade | Duração |
|-------|-----------|---------|
| 00:00 | Reconexão + aluno resume sessão 03 | 15 min |
| 00:15 | Campo Zoom: conceito, configuração, demonstração | 30 min |
| 00:45 | AutoComplete + técnica Pai × Filho (slides + live coding) | 30 min |
| 01:15 | ☕ Intervalo | 10 min |
| 01:25 | Select com Dataset (populando combos dinamicamente) | 25 min |
| 01:50 | Listas dinâmicas (tablein/tableout) — live coding | 35 min |
| 02:25 | ☕ Intervalo | 10 min |
| 02:35 | Exercício: Formulário com Zoom + Lista dinâmica | 45 min |
| 03:20 | Revisão + discussão arquitetural sobre performance | 25 min |
| 03:45 | Encerramento + preview do Projeto Integrador | 15 min |

#### 🐛 Armadilhas Clássicas & Troubleshooting
- Zoom não retorna dados → Verificar dataset vinculado e campos de retorno
- Pai × Filho sem limpar campo filho → Dado inconsistente
- Lista dinâmica perde dados ao salvar → Verificar nomes dos campos com prefixo `tablein_` / `tableout_`
- Performance ruim com muitos registros no Zoom → Usar filtro e paginação

#### 🎁 Desafio Extra (Devs Avançados)
- Implementar validação que impede salvar o formulário se a lista dinâmica estiver vazia

---

### 📒 SESSÃO 05 — Projeto Integrador I — Início do Projeto Fluig
**Data:** 31/03/2026 (Segunda) · **Bloco:** 🟨 PROJETO

**Objetivo:** Aplicar tudo que foi visto (sessões 01-04) construindo o formulário e a estrutura inicial do Sistema de Solicitação de Compras.

#### Conteúdo Programático
- Definição do escopo: Sistema de Solicitação de Compras
- Análise de requisitos simplificada (campos, regras, fluxo)
- Criação do formulário base (dados do solicitante, itens, valores)
- Lista dinâmica de itens da compra
- Estrutura inicial do processo (rascunho do fluxo BPMN)
- Deploy e testes do formulário

#### 🎯 Estratégia do Instrutor
- **Sessão 100% hands-on.** Pouquíssimos slides. A turma constrói junto.
- **Comece pelo "papel":** Antes de abrir o Eclipse, discutir com a turma: "Que campos precisa ter? Quem aprova? Qual a regra?" 15-20 min de discussão que economiza retrabalho.
- **Construção em par:** Devs e analistas trabalham em dupla. Analista define campos, dev implementa.
- **Ganho de tempo:** Ter o formulário base pré-pronto como backup. Se a turma travar, cola o código e continua narrando.

#### ⏱️ Timing Detalhado
| Tempo | Atividade | Duração |
|-------|-----------|---------|
| 00:00 | Reconexão + aluno resume sessão 04 | 10 min |
| 00:10 | Apresentação do Projeto Integrador: escopo e requisitos | 20 min |
| 00:30 | Discussão coletiva: campos, regras, fluxo de aprovação | 20 min |
| 00:50 | Live coding: Criação do formulário base (cabeçalho) | 30 min |
| 01:20 | ☕ Intervalo | 10 min |
| 01:30 | Live coding: Lista dinâmica de itens + cálculo de total | 40 min |
| 02:10 | Exercício autônomo: Cada um melhora/personaliza seu form | 30 min |
| 02:40 | ☕ Intervalo | 10 min |
| 02:50 | Deploy + testes coletivos (preencher, salvar, verificar dados) | 30 min |
| 03:20 | Rascunho do fluxo BPMN no quadro (preparação para sessão 06) | 25 min |
| 03:45 | Encerramento + preview de BPMN | 15 min |

#### 🐛 Armadilhas Clássicas & Troubleshooting
- Esquecer o cálculo automático do total → JavaScript que soma itens
- Estrutura de `name` inconsistente → Definir convenção ANTES de começar
- Deploy demora → Usar ctrl+shift+F9 para deploy rápido no Studio

#### 🎁 Desafio Extra (Devs Avançados)
- Adicionar campo de upload de anexo (cotação) ao formulário

---

### 📕 SESSÃO 06 — Workflows e BPMN 2.0 — Fundamentos
**Data:** 03/04/2026 (Quinta) · **Bloco:** 🟪 WORKFLOWS

**Objetivo:** Modelagem correta de processos com BPMN 2.0 e configuração de workflows no Fluig.

#### Conteúdo Programático
- Conceitos de BPM e por que BPMN 2.0
- Notação BPMN no Fluig: atividades, eventos, gateways
- Elementos: Sinal, Timer, Gateway (exclusivo, paralelo, inclusivo), Link
- Fluxo padrão e automático
- Configurações do Painel de Controle
- Vinculação de formulário ao processo

#### 🎯 Estratégia do Instrutor
- **Analogia do mundo real:** Antes de abrir o Fluig, mapear um processo do dia a dia (ex: pedir pizza por app) em BPMN no quadro. Turma participa. 20 min que engajam muito.
- **Gateway é o divisor de águas:** Gaste tempo aqui. A diferença entre exclusivo, paralelo e inclusivo confunde até devs experientes. Use exemplos concretos.
- **Erro proposital:** Criar um gateway exclusivo sem condição em uma das saídas → processo trava → diagnosticar → corrigir.
- **Conectar com o Projeto Integrador:** "Lembram do rascunho que fizemos na sessão 05? Agora vamos implementar de verdade."

#### ⏱️ Timing Detalhado
| Tempo | Atividade | Duração |
|-------|-----------|---------|
| 00:00 | Reconexão + aluno resume sessão 05 | 15 min |
| 00:15 | BPM conceitual + BPMN no quadro (exercício da pizza) | 25 min |
| 00:40 | Notação BPMN no Fluig: elementos fundamentais (slides) | 30 min |
| 01:10 | ☕ Intervalo | 10 min |
| 01:20 | Gateways em profundidade (exclusivo × paralelo × inclusivo) | 35 min |
| 01:55 | Live coding: Primeiro workflow no Fluig (linear simples) | 30 min |
| 02:25 | ☕ Intervalo | 10 min |
| 02:35 | Exercício: Workflow com gateway exclusivo (aprovação/rejeição) | 40 min |
| 03:15 | Painel de Controle + fluxo automático | 20 min |
| 03:35 | Encerramento + preview de eventos e SDK | 25 min |

#### 🐛 Armadilhas Clássicas & Troubleshooting
- Gateway sem condição → processo trava em runtime
- Atividade sem destinatário → erro ao enviar tarefa
- Esquecer de vincular formulário ao processo → formulário não abre na tarefa
- Timer com formato de data errado → não dispara

#### 🎁 Desafio Extra (Devs Avançados)
- Criar workflow com gateway paralelo (duas aprovações simultâneas que convergem)

---

### 📕 SESSÃO 07 — Workflows Avançados e Eventos de Formulário
**Data:** 07/04/2026 (Segunda) · **Bloco:** 🟪 WORKFLOWS

**Objetivo:** Automatizar processos complexos com eventos de formulário e classes do SDK.

#### Conteúdo Programático
- Elementos condicionais, finais e múltiplos no BPMN
- Eventos de formulário: `beforeTaskSave`, `afterTaskSave`, `beforeTaskComplete`, `afterTaskComplete`, `displayFields`, `enableFields`, `validateForm`
- Boas práticas de uso de eventos (o que vai em cada um e por quê)
- Classes internas do fluig SDK (hAPI, fluigAPI, DatasetFactory)
- Manipulação de dados do processo via JavaScript

#### 🎯 Estratégia do Instrutor
- **Mapa de eventos:** Criar um diagrama mostrando QUANDO cada evento dispara no ciclo de vida de uma tarefa. Esse diagrama é o material mais consultado depois do treinamento.
- **"Onde colocar minha lógica?"** — Essa é a pergunta de ouro. Criar uma tabela comparativa: validação → `validateForm`; esconder campos → `displayFields`; gravar log → `afterTaskSave`. Gastar 20 min aqui.
- **Erro proposital:** Colocar lógica pesada no `beforeTaskSave` → formulário fica lento → mover para `afterTaskComplete` → explicar a diferença.
- **SDK como canivete suíço:** Demonstrar hAPI para mover processo automaticamente e DatasetFactory para consultar dados dentro do evento.

#### ⏱️ Timing Detalhado
| Tempo | Atividade | Duração |
|-------|-----------|---------|
| 00:00 | Reconexão + aluno resume sessão 06 | 15 min |
| 00:15 | Elementos avançados BPMN (condicionais, finais, múltiplos) | 25 min |
| 00:40 | Mapa de eventos do formulário (ciclo de vida completo) | 30 min |
| 01:10 | ☕ Intervalo | 10 min |
| 01:20 | Live coding: Implementando eventos (displayFields + validateForm) | 35 min |
| 01:55 | Live coding: beforeTaskSave + afterTaskComplete | 25 min |
| 02:20 | ☕ Intervalo | 10 min |
| 02:30 | Introdução ao SDK (hAPI, fluigAPI, DatasetFactory) | 25 min |
| 02:55 | Exercício: Automação no Projeto Integrador (validar valor + mover tarefa) | 40 min |
| 03:35 | Revisão + discussão "onde colocar cada lógica" | 15 min |
| 03:50 | Encerramento + preview sessão 08 | 10 min |

#### 🐛 Armadilhas Clássicas & Troubleshooting
- `beforeTaskSave` síncrono → lógica pesada trava o formulário
- `displayFields` não esconde — altera visibilidade. Dado ainda está lá (segurança!)
- `validateForm` retorna `throw` para bloquear — não retorna `false` (erro comum)
- hAPI.moveTo() com atividade inexistente → erro silencioso

#### 🎁 Desafio Extra (Devs Avançados)
- Implementar regra: se valor > R$10.000, processo vai para diretoria; senão, gerente aprova

---

### 📒 SESSÃO 08 — Projeto Integrador II — Evolução do Projeto
**Data:** 10/04/2026 (Quinta) · **Bloco:** 🟨 PROJETO

**Objetivo:** Integrar workflow ao formulário do Projeto Integrador, aplicando eventos e automações.

#### Conteúdo Programático
- Melhoria do workflow de Solicitação de Compras (adicionar aprovações)
- Implementação de eventos de formulário no projeto
- Regras de negócio: aprovação por alçada de valor
- Integração entre formulário e processo (dados fluindo entre etapas)
- Testes de ponta a ponta

#### 🎯 Estratégia do Instrutor
- **Sessão 100% prática.** A turma evolui o projeto.
- **Code review coletivo:** Pegar o código de 2-3 alunos e revisar na frente de todos. Isso gera discussão rica e ensina boas práticas.
- **Cenário de erro real:** Simular um processo travado e diagnosticar junto com a turma usando o Painel de Controle.
- **Ganho de tempo:** Ter o workflow pré-modelado como backup. Se turma travar no BPMN, colar e focar nos eventos.

#### ⏱️ Timing Detalhado
| Tempo | Atividade | Duração |
|-------|-----------|---------|
| 00:00 | Reconexão + revisão do estado atual do projeto | 15 min |
| 00:15 | Evolução do workflow (adicionar gateway de alçada) | 35 min |
| 00:50 | Implementação de displayFields + enableFields por etapa | 30 min |
| 01:20 | ☕ Intervalo | 10 min |
| 01:30 | Implementação de validateForm + beforeTaskSave | 35 min |
| 02:05 | Implementação de afterTaskComplete (mover tarefa automático) | 25 min |
| 02:30 | ☕ Intervalo | 10 min |
| 02:40 | Testes de ponta a ponta (fluxo completo) | 30 min |
| 03:10 | Code review coletivo (2-3 alunos) | 25 min |
| 03:35 | Troubleshooting de processos travados | 15 min |
| 03:50 | Encerramento + preview de Datasets | 10 min |

#### 🐛 Armadilhas Clássicas & Troubleshooting
- Processo trava sem erro → Verificar condições do gateway (campo em branco = sem match)
- Eventos executam na ordem errada → Revisar ciclo de vida
- Dados não persistem entre etapas → Verificar `name` dos campos
- Tarefa vai para usuário errado → Verificar mecanismo de atribuição

#### 🎁 Desafio Extra (Devs Avançados)
- Implementar notificação por email quando valor ultrapassa limiar

---

### 📙 SESSÃO 09 — Datasets Customizados e Introdução a Integrações REST
**Data:** 14/04/2026 (Segunda) · **Bloco:** 🟧 DADOS

**Objetivo:** Dominar a criação e consumo de datasets customizados, entender jornalização e performance, e preparar o terreno para integração REST com Protheus.

#### Conteúdo Programático
- O que é um Dataset e por que é o coração do Fluig
- Datasets internos vs. customizados
- Desenvolvimento de datasets customizados (função `createDataset`)
- Anatomia completa: `addColumn`, `addRow`, constraints, filtros
- Consumo de datasets via JavaScript (formulário) e via DatasetFactory (eventos)
- Datasets jornalizados (cache) e offline — quando usar cada um
- Otimização de performance (constraints, filtros, paginação)
- Discussão arquitetural: "Dataset vs chamada REST direta — quando usar cada um?"
- Preview: como um dataset pode encapsular uma chamada REST (preparação sessão 10)

#### 🎯 Estratégia do Instrutor
- **Dataset como "view do banco":** Analogia para devs — dataset é como uma view SQL, mas com esteroides (cache, filtros, constraints). Para analistas: "é a forma do Fluig buscar e organizar informações."
- **Construir do zero, passo a passo:** Diferente da Eletronacional (datasets prontos), aqui a turma vai CRIAR. Ir devagar no `createDataset`, explicar cada linha.
- **Performance ao vivo:** Criar um dataset sem filtro que retorna 10.000 registros → mostrar a lentidão → adicionar constraints → mostrar a melhoria.
- **Jornalização como decisão arquitetural:** "Fornecedores mudam todo dia? Não. Então jornaliza. Estoque muda toda hora? Sim. Então não jornaliza." — análise de trade-offs com exemplos do Projeto Integrador.
- **Quando Dataset vs API REST direta?** — Essa discussão é OURO. Dataset quando precisa de cache/reuso/jornalização, API direta quando precisa de tempo real e é uso único. 20 min de discussão que prepara a sessão 10.
- **Ganho de tempo:** A discussão "quando usar o quê" rende fácil 20 min. E a demo de performance (com/sem filtro) impressiona e consome tempo naturalmente.

#### ⏱️ Timing Detalhado
| Tempo | Atividade | Duração |
|-------|-----------|---------|
| 00:00 | Reconexão + aluno resume sessão 08 | 15 min |
| 00:15 | Conceito de Dataset + datasets internos do Fluig (slides + demo navegando) | 25 min |
| 00:40 | Live coding narrado: Primeiro dataset customizado do zero | 30 min |
| 01:10 | ☕ Intervalo | 10 min |
| 01:20 | Consumo de datasets: JS no formulário + DatasetFactory em eventos | 30 min |
| 01:50 | Exercício guiado: Dataset que lista departamentos (dado fixo) | 20 min |
| 02:10 | ☕ Intervalo | 10 min |
| 02:20 | Jornalização e offline: conceito, configuração, trade-offs | 25 min |
| 02:45 | Demo de performance: com/sem constraints + filtros | 20 min |
| 03:05 | Discussão arquitetural: "Dataset vs REST direta — quando usar cada um?" | 20 min |
| 03:25 | Preview da sessão 10: "E se o dataset chamasse uma API externa?" | 10 min |
| 03:35 | Encerramento + tarefa mental: "pensem quais dados do Protheus o projeto precisa" | 25 min |

#### 🐛 Armadilhas Clássicas & Troubleshooting
- `createDataset` retorna `null` → Verificar se retorna `dataset` no final da função
- Dataset sem constraint retorna tudo → Problema grave de performance em produção
- Jornalização com dados que mudam frequentemente → Cache desatualizado, usuário vê dado velho
- Erro "dataset not found" → Verificar nome exato (case sensitive!)
- `addColumn` com tipo errado → Dados truncados ou incorretos
- Constraint com campo inexistente → Retorna vazio sem erro (silencioso)

#### 🎁 Desafio Extra (Devs Avançados)
- Criar dataset que consulta outro dataset internamente (encadeamento de datasets)

---

### 📙 SESSÃO 10 — Integração REST com TOTVS Protheus (Apenas Consultas)
**Data:** 17/04/2026 (Quinta) · **Bloco:** 🟧 DADOS

**Objetivo:** Construir do zero datasets que consomem a API REST do Protheus via GET, entender autenticação Basic Auth, tratar erros, e integrar os dados ao Projeto Integrador (Zoom de Fornecedores, Produtos, Centros de Custo).

> ⚠️ **ESCOPO IMPORTANTE:** Esta sessão cobre APENAS consultas (método GET). Não abordamos POST, PUT ou DELETE. A razão arquitetural: o Fluig é o dono do processo, o Protheus é a fonte de dados mestre. O Fluig consulta, não grava no ERP.

> ⚠️ **PRÉ-REQUISITO DO INSTRUTOR:** Antes desta sessão, confirmar com o cliente: (1) URL base da API REST do Protheus, (2) credenciais Basic Auth, (3) quais endpoints estão disponíveis (SA2, SB1, CTT, SE4). Ter um plano B com mock/JSON estático caso o ambiente esteja fora.

#### Conteúdo Programático
- Arquitetura da integração: Fluig → Dataset → API REST Protheus → GET → JSON → Dataset
- Autenticação Basic Auth no Protheus (como funciona, onde configurar)
- **Dataset 1 — Fornecedores (SA2):** Construção ao vivo, passo a passo
- **Dataset 2 — Produtos (SB1):** Construção guiada (turma acompanha)
- **Dataset 3 — Centros de Custo (CTT):** Exercício autônomo da turma
- Tratamento de erros em integrações (timeout, 401 Unauthorized, 500 Internal, servidor fora)
- Consumo dos datasets no Projeto Integrador: campo Zoom de Fornecedor + Zoom de Produto
- Quando jornalizar um dataset de integração (Fornecedores sim, Estoque não)
- Menção ao SOAP e Wizard de Integração (conceitual, sem hands-on profundo)

#### 🎯 Estratégia do Instrutor

- **"Do isolado ao conectado":** Abrir a sessão mostrando o Projeto Integrador atual (formulário com dados manuais) → depois mostrar o mesmo formulário puxando dados do Protheus. O contraste é poderoso.

- **Construção progressiva (3 datasets, 3 níveis de autonomia):**
  - Dataset SA2 (Fornecedores): **instrutor constrói ao vivo, narrando cada linha.** Turma assiste e acompanha. É o mais didático — explica `java.net.URL`, `openConnection`, `getInputStream`, parse de JSON, `addColumn`/`addRow`.
  - Dataset SB1 (Produtos): **instrutor guia, turma implementa.** Mesma estrutura do SA2 mas com campos diferentes. Turma já entendeu o padrão.
  - Dataset CTT (Centros de Custo): **exercício autônomo.** Turma faz sozinha com base no padrão aprendido. Instrutor circula e ajuda.

- **Erro proposital com autenticação:** Montar o dataset SA2 sem o header Authorization → mostrar o 401 → explicar Basic Auth → corrigir → funciona. Isso ensina debugging de integração.

- **Demo de erros é obrigatória:** Desligar o mock/apontar para URL errada → timeout. Mudar a senha → 401. Pedir campo inexistente → 500. A turma PRECISA ver esses cenários antes de ir pra produção.

- **SOAP e Wizard como menção (15-20 min máx):** Explicar conceitualmente que existe SOAP para APIs legadas e o Wizard como atalho visual. Não aprofundar — o foco é REST que é o padrão moderno.

- **Plano B (ambiente fora):** Ter JSONs estáticos de resposta do Protheus salvos localmente. Se o ambiente cair, o dataset lê do JSON em vez da API. A turma aprende a mesma coisa e você não perde a sessão.

- **Ganho de tempo natural:** A construção narrada do primeiro dataset (SA2) consome fácil 40 min. A demo de erros rende 25 min. A discussão "quando jornalizar integração" rende 15 min. Sessão se preenche sozinha.

#### Datasets do Projeto Integrador — Mapeamento Protheus

| Dataset Fluig | Endpoint Protheus (REST GET) | Campos Retornados | Uso no Projeto |
|---------------|------------------------------|-------------------|----------------|
| `ds_fornecedores` | `/api/framework/v1/consultSQL` ou `/rest/SA2` | A2_COD, A2_LOJA, A2_NOME, A2_CGC, A2_MUN | Zoom no formulário (selecionar fornecedor) |
| `ds_produtos` | `/api/framework/v1/consultSQL` ou `/rest/SB1` | B1_COD, B1_DESC, B1_UM, B1_PRV1, B1_GRUPO | Zoom na lista dinâmica (selecionar produto/item) |
| `ds_centros_custo` | `/api/framework/v1/consultSQL` ou `/rest/CTT` | CTT_CUSTO, CTT_DESC01 | Select no formulário (centro de custo) |
| `ds_cond_pagamento` | `/api/framework/v1/consultSQL` ou `/rest/SE4` | E4_CODIGO, E4_DESCRI, E4_COND | Select no formulário (condição: à vista, 30/60/90) |

> **Nota sobre endpoints:** A URL exata depende da configuração do Protheus do cliente. O padrão mais comum é via `consultSQL` (query genérica) ou via entidade REST direta. Confirmar com TI do cliente qual está disponível.

#### ⏱️ Timing Detalhado
| Tempo | Atividade | Duração |
|-------|-----------|---------|
| 00:00 | Reconexão + aluno resume sessão 09 | 10 min |
| 00:10 | Contexto: "Do isolado ao conectado" — antes/depois no Projeto Integrador | 10 min |
| 00:20 | Arquitetura da integração (slides): Fluig → Dataset → REST → Protheus | 15 min |
| 00:35 | Autenticação Basic Auth: conceito + como montar o header | 10 min |
| 00:45 | **Live coding narrado: Dataset SA2 (Fornecedores) do zero** | 40 min |
| 01:25 | ☕ Intervalo | 10 min |
| 01:35 | **Demo de erros ao vivo:** 401 (auth), timeout, 500, servidor fora | 25 min |
| 02:00 | **Live coding guiado: Dataset SB1 (Produtos) — turma acompanha** | 25 min |
| 02:25 | ☕ Intervalo | 10 min |
| 02:35 | **Exercício autônomo: Dataset CTT (Centros de Custo) — turma faz sozinha** | 25 min |
| 03:00 | Integração no Projeto: conectar Zoom de Fornecedor ao ds_fornecedores | 20 min |
| 03:20 | Discussão: "Quando jornalizar dataset de integração?" | 10 min |
| 03:30 | Menção: SOAP e Wizard de Integração (conceitual) | 15 min |
| 03:45 | Encerramento + preview de Portais | 15 min |

#### 🐛 Armadilhas Clássicas & Troubleshooting
- **401 Unauthorized** → Header Authorization ausente ou credencial errada. Basic Auth = `Base64("usuario:senha")`. Erro mais comum da sessão.
- **Timeout / Connection refused** → URL errada, firewall bloqueando, Protheus fora. Sempre testar a URL no browser/Postman antes.
- **JSON parse error** → API retornando HTML de erro (página de login) em vez de JSON. Verificar Content-Type da resposta.
- **Dataset retorna vazio** → API respondeu mas sem registros no filtro. Verificar query/parâmetros.
- **CORS** → Não se aplica! Dataset roda no SERVIDOR Fluig (Java/Rhino), não no browser. Se alguém perguntar, explicar essa distinção — é um ponto arquitetural importante.
- **Encoding de caracteres** → Nomes com acentos vindos do Protheus podem vir quebrados. Verificar charset UTF-8 na leitura do InputStream.
- **Campo inexistente no retorno** → O dataset monta `addRow` com posição fixa. Se a API muda a estrutura, quebra silenciosamente.

#### 🎁 Desafio Extra (Devs Avançados)
- Criar o dataset SE4 (Condições de Pagamento) sozinho E integrar como Select no formulário do Projeto Integrador

#### 📦 Material de Backup (Instrutor)
- [ ] JSONs estáticos de mock: `mock_SA2.json`, `mock_SB1.json`, `mock_CTT.json`, `mock_SE4.json`
- [ ] Datasets prontos (código completo) para colar se ambiente falhar
- [ ] Print de tela do Postman testando cada endpoint (para slides)
- [ ] Credenciais Basic Auth do cliente confirmadas

---

### 📕 SESSÃO 11 — Portais, Pages e Widgets
**Data:** 24/04/2026 (Quinta) · **Bloco:** 🟥 PORTAIS

**Objetivo:** Criação de portais completos, seguros e escaláveis dentro do Fluig.

#### Conteúdo Programático
- Desenvolvendo soluções com Forms e Pages
- Portal público com Pages e Forms integrando a processo BPM (SEM código)
- Portais com layouts customizados (WCM)
- Internacionalização de formulários e processos (PT / EN / ES)
- Como consultar Datasets em Portais (boas práticas e segurança)

#### 🎯 Estratégia do Instrutor
- **"Sem código" primeiro:** Mostrar o poder do Pages + Forms sem escrever uma linha. Analistas se iluminam aqui. Depois mostrar a versão com código para os devs.
- **Internacionalização impressiona:** Trocar o idioma ao vivo e ver tudo mudar. Demo rápida mas de alto impacto.
- **Segurança é sério:** "Nunca exponha datasets sensíveis em portais públicos." Mostrar como um dataset sem controle de acesso pode vazar dados.
- **Layout WCM como diferencial:** Quem domina WCM customizado se destaca no mercado Fluig. Mostrar exemplos reais.

#### ⏱️ Timing Detalhado
| Tempo | Atividade | Duração |
|-------|-----------|---------|
| 00:00 | Reconexão + aluno resume sessão 10 | 15 min |
| 00:15 | Forms e Pages: conceito + portal sem código (demo completa) | 35 min |
| 00:50 | Exercício: Cada um cria um portal simples com Forms + Pages | 25 min |
| 01:15 | ☕ Intervalo | 10 min |
| 01:25 | Layouts customizados WCM (HTML/CSS/JS no portal) | 35 min |
| 02:00 | Internacionalização (i18n) — configuração + demo | 20 min |
| 02:20 | ☕ Intervalo | 10 min |
| 02:30 | Consulta de Datasets em Portais (boas práticas + segurança) | 30 min |
| 03:00 | Exercício: Portal para o Projeto Integrador | 30 min |
| 03:30 | Encerramento + preview de Widgets | 30 min |

#### 🐛 Armadilhas Clássicas & Troubleshooting
- Portal público acessando dataset privado → Erro 403 silencioso
- Layout WCM não renderiza → Verificar estrutura de pastas obrigatória
- i18n não funciona → Verificar nomenclatura dos arquivos de tradução
- CSS do portal conflita com Style Guide → Usar namespacing

#### 🎁 Desafio Extra (Devs Avançados)
- Criar layout WCM responsivo com menu dinâmico baseado em dataset

---

### 📕 SESSÃO 12 — Widgets Avançados e Projeto Final
**Data:** 28/04/2026 (Segunda) · **Bloco:** 🟥 PORTAIS

**Objetivo:** Finalizar uma solução completa no Fluig integrando todos os pilares: formulários, processos, datasets, widgets e portais.

#### Conteúdo Programático
- Templates e bibliotecas externas em portais
- Eventos binding em widgets (ações do usuário)
- Armazenamento de parâmetros por data param (salvar preferências)
- Gráficos com fluig Style Guide (Chart.js integrado)
- Manipulação de datasets a partir de widgets
- **Projeto Integrador V — Finalização completa**
- Retrospectiva e próximos passos

#### 🎯 Estratégia do Instrutor
- **Sessão de "amarrar tudo":** O widget final consulta o dataset, que integra com Protheus, que alimenta o gráfico, que está no portal. Full circle.
- **Gráfico como grand finale:** Widget com gráfico de "total de compras por mês" consumindo dados reais do Projeto Integrador. Visual + impactante.
- **Retrospectiva com valor:** Últimos 30 min — repassar tudo que foi construído, mostrar o Projeto Integrador completo, celebrar com a turma.
- **Próximos passos concretos:** Indicar o que estudar depois, como se manter atualizado, e como a JYNX pode ajudar a resolver problemas futuros.

#### ⏱️ Timing Detalhado
| Tempo | Atividade | Duração |
|-------|-----------|---------|
| 00:00 | Reconexão + aluno resume sessão 11 | 10 min |
| 00:10 | Templates e bibliotecas externas | 20 min |
| 00:30 | Eventos binding em widgets (live coding) | 25 min |
| 00:55 | Data param: salvando preferências do usuário | 15 min |
| 01:10 | ☕ Intervalo | 10 min |
| 01:20 | Gráficos com Style Guide (Chart.js no widget) | 30 min |
| 01:50 | Manipulação de datasets via widget | 20 min |
| 02:10 | ☕ Intervalo | 10 min |
| 02:20 | **PROJETO INTEGRADOR FINAL:** Construção do widget dashboard | 50 min |
| 03:10 | Apresentação dos projetos finalizados | 20 min |
| 03:30 | Retrospectiva: tudo que construímos em 48h | 15 min |
| 03:45 | Próximos passos + certificado + encerramento | 15 min |

#### 🐛 Armadilhas Clássicas & Troubleshooting
- Widget não renderiza gráfico → Verificar se Chart.js foi importado corretamente
- Data param não salva → Verificar se widget está em modo edição
- Binding não dispara → Verificar seletor jQuery e contexto do widget
- Dataset retorna vazio no widget → Verificar permissões do dataset

#### 🎁 Desafio Extra (Devs Avançados)
- Adicionar filtro interativo no gráfico (por período, por departamento)

---

## TOOLKIT DO INSTRUTOR — TÉCNICAS DE GESTÃO DE TEMPO

### Técnicas Para "Ganhar Tempo" de Forma Inteligente

| Técnica | Quando Usar | Tempo Ganho |
|---------|-------------|-------------|
| **Recapitulação Colaborativa** | Início de toda sessão | 10-15 min |
| **Erro Proposital** | Antes de cada conceito novo | 10-15 min |
| **Pergunta Devolvida** | Quando alguém pergunta algo complexo | 5-10 min |
| **Comparativo Arquitetural** | Após apresentar uma solução | 10-15 min |
| **Live Coding Narrado** | Toda demonstração de código | +15 min vs cola direta |
| **Code Review Coletivo** | Sessões de projeto (05, 08, 12) | 20-30 min |
| **Discussão "Quando usar X vs Y"** | Sessões 09, 10, 11 | 15-20 min |
| **Troubleshooting ao vivo** | Quando alguém tem erro | 10-20 min |

### Colas e Buffers de Segurança

1. **"Materiais de estudo"** (sessão 01) — expandível de 10 a 30 min
2. **Troubleshooting de ambiente** (sessão 02) — naturalmente variável
3. **Discussão de requisitos do Projeto** (sessão 05) — controlável
4. **REST vs SOAP debate** (sessão 10) — pode render uma aula inteira
5. **Retrospectiva** (sessão 12) — expandível conforme necessário

### Frases-Chave Para Ganhar Tempo Naturalmente

- "Antes de eu mostrar a solução, o que vocês fariam?" (abre discussão)
- "Alguém já passou por esse problema na empresa? Como resolveram?" (experiência real)
- "Vou mostrar primeiro o jeito errado, pra vocês entenderem por que fazemos diferente" (erro proposital)
- "Isso é uma decisão arquitetural importante. Vamos pensar nos prós e contras." (análise profunda)
- "Quem quer tentar explicar o que esse código faz?" (turma ensina turma)

---

## DEPENDÊNCIAS ENTRE SESSÕES (Pirâmide)

```
Sessão 12: Widgets Avançados + Projeto Final
    └── Sessão 11: Portais e Pages
        └── Sessão 10: Integrações ERP
            └── Sessão 09: Datasets
                └── Sessão 08: Projeto Integrador II
                    └── Sessão 07: Workflows Avançados + Eventos
                        └── Sessão 06: BPMN 2.0 Fundamentos
                            └── Sessão 05: Projeto Integrador I
                                └── Sessão 04: Formulários Avançados
                                    └── Sessão 03: Formulários Fundamentos
                                        └── Sessão 02: Ambiente + Style Guide
                                            └── Sessão 01: Introdução + Arquitetura
```

---

## PRÓXIMOS ENTREGÁVEIS

Após aprovação deste roteiro macro, serão produzidos por sessão:
1. **Códigos prontos** (copiar/colar) — arquivos .js, .html, .xml organizados por sessão
2. **Apresentação PPTX** — identidade visual JYNX (laranja #E85D04, Trebuchet MS)
3. **README de cola rápida** — troubleshooting, comandos, atalhos por sessão

---

## CHECKLIST PRÉ-TREINAMENTO (Ações do Instrutor)

### Antes da Sessão 01 (17/03)
- [ ] Confirmar ambiente Fluig disponível e acessível para todos os alunos
- [ ] Verificar versão do Java compatível com Eclipse/plugin
- [ ] Preparar projeto "Hello Fluig" de exemplo
- [ ] Ter o Projeto Integrador finalizado como "trailer" da sessão 01

### Antes da Sessão 10 (17/04) — CRÍTICO
- [ ] Confirmar com TI do cliente: URL base da API REST do Protheus
- [ ] Obter credenciais Basic Auth (usuário + senha)
- [ ] Testar no Postman cada endpoint: SA2, SB1, CTT, SE4
- [ ] Confirmar quais endpoints estão disponíveis (consultSQL genérico ou entidade REST?)
- [ ] Preparar JSONs de mock (plano B se ambiente cair)
- [ ] Criar datasets prontos como backup para colar no dia
- [ ] Verificar se o servidor Fluig consegue alcançar o Protheus (firewall, rede)

### Antes de Cada Sessão
- [ ] Código backup pronto (versão "antes" e "depois" do exercício)
- [ ] Slides atualizados
- [ ] README de cola revisado
- [ ] Testar todos os deploys no ambiente

---

*Documento gerado para o treinamento JYNX — Desenvolvimento Fluig Avançado (48h)*
*Versão 1.1 — Março 2026*
*Atualização: Escopo de integração definido (apenas GET), datasets a serem criados do zero, consultas mapeadas (SA2, SB1, CTT, SE4)*