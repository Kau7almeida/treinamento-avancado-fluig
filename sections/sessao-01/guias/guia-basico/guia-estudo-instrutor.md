# 📚 Guia de Estudo do Instrutor — Sessão 01
## Introdução, Ferramentas e Boas Práticas
### Treinamento Avançado Fluig | JYNX

---

## 🎯 Ordem Sugerida de Estudo

Leia este material **na noite anterior** à aula. Tempo estimado: 45-60 minutos.

| Ordem | O que estudar | Tempo | Prioridade |
|-------|--------------|-------|------------|
| 1 | Glossário de termos (final deste doc) | 5 min | Essencial |
| 2 | Arquitetura do Fluig (seção completa) | 15 min | Essencial |
| 3 | Rhino JS — por que ES5? (seção completa) | 10 min | Essencial |
| 4 | Perguntas difíceis e como responder | 10 min | Essencial |
| 5 | Erros propositais preparados | 5 min | Recomendado |
| 6 | Abrir o fluig Style Guide e navegar 10 min | 10 min | Recomendado |
| 7 | Abrir o Projeto Integrador finalizado e praticar a navegação | 5 min | Se tiver tempo |

**Dica:** Se tiver apenas 20 minutos, leia os itens 1, 2 e 4. São os que mais impactam sua confiança em sala.

---

## 🏗️ Arquitetura do Fluig — Explicação Profunda

### O que é o Fluig na essência?

O Fluig é uma **plataforma corporativa da TOTVS** que roda em cima de um servidor de aplicação Java chamado **WildFly** (antigo JBoss). Ele não é um único produto — é um ecossistema com 4 módulos integrados:

- **ECM** (Enterprise Content Management) — gestão de documentos e formulários
- **BPM** (Business Process Management) — motor de workflows
- **WCM** (Web Content Management) — portais e páginas web
- **Integração** — datasets, APIs REST/SOAP, conexão com ERPs

**Por que isso importa para o instrutor:** Quando você explica que "tudo roda no mesmo servidor", o aluno entende por que formulário, processo e dataset conversam entre si sem precisar de API intermediária. São módulos dentro do mesmo runtime Java.

### As 3 camadas de execução

Esse é o conceito mais importante da sessão inteira. Se o aluno sair entendendo isso, a sessão foi um sucesso.

#### Camada 1 — Browser (Cliente)

**O que é:** Tudo que roda no navegador do usuário.

**O que executa aqui:**
- Scripts dentro de tags `<script>` nos formulários HTML
- JavaScript de widgets
- CSS do Style Guide
- jQuery para manipulação de DOM
- Validações visuais, máscaras de campo, interações do usuário

**Engine:** V8 (Chrome), SpiderMonkey (Firefox) — JavaScript moderno ES6+.

**Tem acesso a:** `document`, `window`, `console`, DOM completo, localStorage, fetch API, tudo que o browser oferece.

**Analogia para a turma:** "É o JavaScript que vocês já conhecem. O mesmo que roda em qualquer site."

#### Camada 2 — Servidor (Rhino JS)

**O que é:** Uma engine JavaScript chamada Mozilla Rhino que roda **dentro da JVM** (Java Virtual Machine), no servidor.

**O que executa aqui:**
- Datasets customizados (função `createDataset`)
- Eventos de formulário: `displayFields`, `enableFields`, `validateForm`, `beforeTaskSave`, `afterTaskSave`, `beforeTaskComplete`, `afterTaskComplete`
- Eventos de processo
- Callbacks de integração

**Engine:** Mozilla Rhino — implementação de JavaScript escrita 100% em Java. Suporta apenas **ES5**.

**NÃO tem acesso a:** `document`, `window`, `console`, DOM, `fetch`, `XMLHttpRequest`, nada do browser.

**TEM acesso a:** Classes Java nativas! `java.net.URL`, `java.util.HashMap`, `java.io.BufferedReader`, etc. E as classes do SDK do Fluig: `hAPI`, `fluigAPI`, `DatasetFactory`.

**Por que isso confunde:** O desenvolvedor escreve JavaScript, mas esse JS roda no servidor dentro do Java. É como se fosse duas linguagens com a mesma sintaxe — mas ambientes completamente diferentes.

**Analogia para a turma:** "Imaginem que existem dois mundos. O mundo do browser, onde tudo é colorido e moderno. E o mundo do servidor, onde as regras são de 2010 — mas em compensação, vocês têm acesso a todo o poder do Java."

#### Camada 3 — Banco de dados + Serviços Java

**O que é:** A camada de persistência e serviços internos do Fluig.

**Banco de dados:** O Fluig suporta PostgreSQL, MySQL, Oracle e SQL Server. O banco armazena:
- Dados de formulários (cada campo `name` vira uma coluna na tabela do formulário)
- Metadados de processos (quem está em qual etapa, histórico)
- Documentos do ECM (metadados, o binário fica no filesystem)
- Configurações da plataforma

**SDK Java disponível no Rhino:**
- `hAPI` — API de workflow. Permite mover processos automaticamente, definir próxima atividade, atribuir tarefa a usuário
- `DatasetFactory` — Permite consultar qualquer dataset de dentro de um evento ou outro dataset
- `fluigAPI` — Acesso a serviços internos: buscar usuários, grupos, papéis, configurações
- `WCMAPI` — Acesso a funcionalidades do portal

**Solr (Apache Solr):** Motor de busca full-text. Indexa documentos, formulários e conteúdos. Quando o usuário pesquisa algo na barra de busca do Fluig, quem processa é o Solr, não uma query SQL.

**O desenvolvedor nunca acessa o banco diretamente.** Tudo é abstraído por datasets e APIs. Se alguém perguntar "posso fazer um SELECT direto?", a resposta é: "Não é a prática recomendada. Use datasets."

### Por que Rhino e não Node.js?

Essa pergunta **vai aparecer**. Esteja preparado.

**Resposta curta:** Porque o Fluig roda dentro de um servidor Java (WildFly), e o Rhino se integra nativamente com a JVM.

**Resposta completa:**

1. **Integração nativa com Java:** O Rhino é uma engine JS escrita em Java. Isso significa que código JS pode instanciar classes Java diretamente — `var url = new java.net.URL("http://...")`. Essa integração é transparente. Com Node.js, seria necessário criar uma ponte entre a JVM e o V8 (que roda em C++), adicionando complexidade e latência.

2. **Sandbox controlado:** O Rhino roda dentro da sandbox da JVM. O Fluig controla exatamente o que o código JS pode fazer — sem acesso a filesystem, sem execução de processos, sem sockets arbitrários. Isso é importante para segurança em ambiente multi-tenant.

3. **Decisão histórica:** O Fluig foi arquitetado no início dos anos 2010, quando Node.js ainda era jovem e o ecossistema Java EE era o padrão para aplicações corporativas. A escolha fez sentido na época e migrar agora seria reescrever a plataforma inteira.

4. **Trade-off:** A desvantagem é ficar preso ao ES5. Sem `let`, `const`, arrow functions, async/await, template literals. O Rhino não acompanhou a evolução do ECMAScript. Na prática, é como programar em JavaScript de 2010 — funciona perfeitamente, mas sem os confortos modernos.

**Se alguém insistir "mas podiam ter migrado":** A resposta honesta é que migrar uma plataforma inteira de Rhino para outro runtime é um esforço gigantesco que provavelmente não se justifica pelo retorno. O Rhino funciona, é estável, e a integração com Java é o superpoder que permite coisas como chamar APIs REST usando `java.net.URL` direto do JS.

### O atributo `name` — o contrato com o banco

Esse conceito parece simples, mas é a fundação de tudo.

Quando você cria um campo HTML com `name="fornecedor"` e o formulário é cadastrado no Fluig:
1. O Fluig cria automaticamente uma coluna chamada `fornecedor` na tabela daquele formulário
2. Quando o usuário preenche e salva, o valor vai para essa coluna
3. Quando um dataset consulta aquele formulário, o campo `fornecedor` aparece como coluna no resultado

**Campo sem `name`:** O Fluig não sabe onde salvar. O dado é preenchido pelo usuário, mas ao salvar, simplesmente some. É o erro #1 mais comum de iniciantes.

**Campo com `name` duplicado:** O segundo valor sobrescreve o primeiro. Comportamento silencioso e difícil de debugar.

**`name` com caracteres especiais:** Espaços, acentos, ou caracteres especiais no `name` podem causar comportamento imprevisível. Use sempre snake_case sem acentos: `nome_completo`, `data_admissao`, `valor_total`.

### Fluig Plataforma vs Fluig Identity

**Fluig Plataforma:** É onde tudo acontece — formulários, processos, datasets, portais, widgets. É o produto principal e o foco do treinamento.

**Fluig Identity:** É um produto **separado** de Single Sign-On (SSO) e gestão de identidade. Permite login único para múltiplas aplicações (Fluig, Protheus, etc).

**Analogia:** O Identity é o "porteiro do prédio" (autentica quem é você). A Plataforma é o "prédio inteiro" (onde o trabalho acontece). São produtos distintos que se integram, mas podem funcionar independentemente.

**No treinamento:** Trabalhamos 100% na Plataforma. Se alguém perguntar sobre Identity, explique a diferença e siga adiante.

---

## ❓ Perguntas Difíceis e Como Responder

### "O Fluig é low-code ou high-code?"

**Resposta:** É os dois. Para analistas, o Fluig oferece recursos low-code — você pode criar processos BPM no editor visual, montar portais com drag-and-drop de widgets, e usar Pages e Forms sem escrever código. Para desenvolvedores, é high-code — formulários customizados em HTML/CSS/JS, datasets com lógica complexa, integrações REST/SOAP, widgets com JavaScript avançado. O poder real do Fluig aparece quando você combina as duas abordagens.

### "Dá para usar React/Vue/Angular nos formulários?"

**Resposta:** Tecnicamente é possível injetar frameworks modernos dentro de widgets, mas não é a abordagem recomendada. O Fluig tem seu próprio ecossistema (Style Guide, jQuery, componentes nativos) e misturar frameworks gera problemas de compatibilidade, performance e manutenção. Use o Style Guide + jQuery — é o caminho suportado oficialmente.

### "Qual banco de dados vocês recomendam?"

**Resposta:** O Fluig suporta PostgreSQL, MySQL, Oracle e SQL Server. A escolha depende da infraestrutura do cliente. Na prática, a maioria dos clientes TOTVS já usa SQL Server (por causa do Protheus) ou PostgreSQL. Para o desenvolvedor, é irrelevante — você nunca escreve SQL diretamente, tudo é abstraído por datasets.

### "O Fluig é cloud ou on-premise?"

**Resposta:** Ambos. Pode rodar nos servidores da empresa (on-premise) ou na nuvem TOTVS. A arquitetura é a mesma — a diferença é quem administra a infraestrutura. Para o desenvolvedor, o código é idêntico.

### "Preciso saber Java para desenvolver no Fluig?"

**Resposta:** Não para o dia a dia. Você escreve JavaScript (ES5 no servidor, ES6+ no browser). Mas entender que existe Java por baixo ajuda a debugar problemas e a usar classes Java quando necessário (ex: `java.net.URL` para integrações). O SDK do Fluig expõe classes Java que você chama do JS sem precisar escrever Java.

### "O que acontece se eu usar `let` ou `const` num dataset?"

**Resposta:** Erro de sintaxe no servidor. O Rhino não entende ES6. O dataset simplesmente não funciona — retorna erro ou null. É um erro silencioso no deploy (o Fluig não valida a sintaxe JS no momento do deploy, só quando executa).

### "E se o aluno perguntar algo que eu não sei?"

**Resposta sugerida:** "Boa pergunta — vou verificar a documentação oficial e trago a resposta confirmada na próxima sessão." Isso é muito mais profissional do que inventar. Anote a pergunta, pesquise, e traga a resposta na sessão seguinte. O aluno respeita honestidade.

---

## 🐛 Erros Propositais Preparados

### Erro 1: Campo sem `name`

**Quando usar:** Ao explicar formulários e persistência de dados (bloco dos 5 pilares).

**O que fazer:**
1. Criar um formulário simples com 2 campos
2. No primeiro campo, colocar `name="nome_completo"`
3. No segundo campo, **não colocar** `name` (só `id`)
4. Deployar, preencher ambos os campos, salvar
5. Abrir o registro salvo — o primeiro campo tem dado, o segundo está vazio

**Explicação para a turma:**
> "Viram? O primeiro campo salvou porque tem `name`. O segundo sumiu. O Fluig usa o `name` como endereço no banco de dados. Sem endereço, o carteiro não sabe onde entregar."

**Tempo ganho:** 10-15 minutos de discussão + demonstração.

### Erro 2: ES6 no Rhino (preparar para sessões futuras, mencionar nesta)

**Quando usar:** Ao explicar a diferença entre browser e servidor.

**O que fazer:**
1. Mostrar um trecho de código com `let`, arrow function
2. Explicar: "Se eu colocar isso num dataset, o que acontece?"
3. Se tiver acesso ao ambiente, demonstrar o erro
4. Se não tiver, mostrar no slide

**Explicação para a turma:**
> "O Rhino olha para esse `let` e não entende. É como falar inglês com alguém que só fala português. A sintaxe é parecida, mas ele não reconhece."

**Tempo ganho:** 5-10 minutos.

### Erro 3: Confundir Fluig Plataforma com Identity

**Quando usar:** Se alguém mencionar login, SSO, ou autenticação.

**O que fazer:**
1. Perguntar: "Alguém sabe a diferença entre Fluig Plataforma e Fluig Identity?"
2. Deixar a turma especular
3. Explicar com a analogia do porteiro/prédio

**Tempo ganho:** 5-10 minutos de discussão.

---

## 📖 Glossário de Termos Técnicos

| Termo | O que é | Por que importa |
|-------|---------|-----------------|
| **JBoss / WildFly** | Servidor de aplicação Java onde o Fluig roda | É o "contêiner" que executa toda a plataforma |
| **JVM** | Java Virtual Machine — ambiente de execução do Java | O Rhino roda dentro da JVM, por isso acessa classes Java |
| **Rhino** | Engine JavaScript da Mozilla, escrita em Java | Executa datasets e eventos no servidor (ES5 only) |
| **ECM** | Enterprise Content Management | Módulo de gestão de documentos e formulários |
| **BPM** | Business Process Management | Módulo de workflows e processos automatizados |
| **BPMN 2.0** | Business Process Model and Notation | Padrão internacional de modelagem de processos |
| **WCM** | Web Content Management | Módulo de portais, páginas e widgets |
| **Dataset** | Função que retorna dados em formato tabular | Forma padrão de buscar/processar dados no Fluig |
| **Widget** | Componente visual reutilizável em portais | Permite criar dashboards, gráficos, listas |
| **Style Guide** | Sistema de design CSS do Fluig (Bootstrap-based) | Padrão visual obrigatório para formulários e portais |
| **hAPI** | API Java para manipulação de workflows | Permite mover processos automaticamente via código |
| **DatasetFactory** | Classe Java para consultar datasets no servidor | Usado dentro de eventos para buscar dados |
| **fluigAPI** | API Java para serviços internos do Fluig | Acessa usuários, grupos, configurações |
| **Solr** | Apache Solr — motor de busca full-text | Processa buscas na plataforma (não é query SQL) |
| **Deploy** | Enviar o código do Eclipse/Studio para o servidor | O artefato só funciona no Fluig após o deploy |
| **Constraint** | Filtro aplicado a um dataset ao consultá-lo | Evita retornar todos os registros (performance) |
| **Jornalização** | Cache automático de dataset no servidor | Evita reconsultar dados que não mudam com frequência |
| **Evento de formulário** | Função JS que executa em momentos específicos do ciclo de vida do formulário | Ex: `beforeTaskSave`, `displayFields`, `validateForm` |
| **Fluig Identity** | Produto separado de SSO (Single Sign-On) | NÃO é a Plataforma — é o serviço de autenticação |
| **Fluig Studio** | Plugin do Eclipse para desenvolvimento Fluig | IDE oficial para criar, editar e deployar artefatos |

---

## 🔑 Conceitos-Chave para Memorizar

1. **O Fluig é um monolito Java** — todos os módulos (ECM, BPM, WCM) rodam no mesmo servidor WildFly. Não são microsserviços.

2. **Existem 2 mundos de JavaScript** — browser (ES6+, DOM) e servidor (Rhino ES5, Java). Nunca confundir.

3. **O atributo `name` é sagrado** — sem ele, dado não persiste. Com ele duplicado, dado é sobrescrito. Com caractere especial, comportamento imprevisível.

4. **Dataset não é tabela** — é uma função que retorna dados em formato tabular. Pode vir do banco, de uma API, de um cálculo.

5. **BPMN 2.0 é padrão internacional** — o conhecimento vale fora do Fluig, em qualquer ferramenta BPM.

6. **Style Guide é obrigatório** — CSS customizado que conflita com o Style Guide quebra em atualizações da plataforma.

7. **Nomenclatura é requisito técnico** — `frm_`, `wkf_`, `ds_`, `wdg_`, `prt_`. Sem padrão, projeto com 200+ artefatos vira caos.

---

*Material de estudo do instrutor — Sessão 01 | Treinamento Avançado Fluig | JYNX*
*Tempo de leitura estimado: 45-60 minutos*