# SESSÃO 01 — Introdução, Ferramentas e Boas Práticas
## Roteiro do Instrutor | 4 horas

---

## OBJETIVO DA SESSÃO
Entendimento sólido da arquitetura Fluig, dos 5 pilares da plataforma e como eles se conectam dentro de uma solução corporativa. Ao final, o aluno sabe onde cada peça se encaixa e consegue navegar na plataforma com confiança.

---

## PREPARAÇÃO (Antes da aula)

### Checklist
- [ ] Ambiente Fluig acessível para todos os alunos (testar login)
- [ ] Projeto Integrador finalizado deployado (para o "trailer")
- [ ] Slides da sessão 01 abertos e testados
- [ ] Links úteis abertos em abas do navegador (TDN, Style Guide, documentação)
- [ ] Quadro branco ou ferramenta de desenho pronta (para mapa mental)

### Material de Apoio
- Slides: `sessao-01/slides.pptx`
- Cola: `sessao-01/README.md`

---

## TIMELINE DETALHADA

---

### ⏱️ [00:00 - 00:20] ABERTURA E APRESENTAÇÕES (20 min)

**O que fazer:**
1. Se apresentar brevemente (quem é, experiência com Fluig, JYNX)
2. Pedir para cada aluno dizer: nome, função, e o que espera do treinamento
3. Anotar as expectativas (vai usar no encerramento da sessão 12)

**Fala sugerida:**
> "Meu nome é Kauã, sou instrutor da JYNX e trabalho com Fluig há X anos. Nas próximas 48 horas, vocês vão sair daqui construindo soluções completas — formulários, processos, integrações com Protheus, portais e widgets. Mas antes, quero conhecer vocês. Me conta: nome, o que faz na empresa, e o que espera desse treinamento."

**Dica de tempo:** Se a turma for grande (8+), limite a 30 segundos por pessoa. Se for pequena (4-5), deixe fluir — constrói rapport.

**⚡ Gatilho de discussão (se precisar de tempo):**
> "Alguém aqui já mexeu no Fluig? Nem que seja só aprovando uma solicitação? O que acharam?"

---

### ⏱️ [00:20 - 00:30] "TRAILER" DO PROJETO INTEGRADOR (10 min)

**O que fazer:**
Abrir o Projeto Integrador finalizado no Fluig e navegar por ele:
1. Mostrar o formulário de Solicitação de Compras preenchido
2. Mostrar o Zoom buscando fornecedores do Protheus
3. Mostrar o processo BPM rodando (tarefa de aprovação)
4. Mostrar o portal com o widget de dashboard

**Fala sugerida:**
> "Isso aqui é o que vocês vão construir ao longo das 12 sessões. Um sistema completo de Solicitação de Compras — formulário inteligente, processo de aprovação, integração com o Protheus, e um dashboard para acompanhar tudo. Parece complexo? É. Mas a gente vai chegar lá tijolo por tijolo."

**Por que isso funciona:** Cria uma "estrela norte" para o treinamento. Toda vez que um conceito novo parecer abstrato, você pode dizer "lembra do Projeto Integrador? É aqui que isso entra."

---

### ⏱️ [00:30 - 01:10] ARQUITETURA DA PLATAFORMA (40 min)

**O que fazer:**
1. **(5 min)** Slide: O que é o Fluig? Posicionamento da TOTVS, diferença entre Fluig Plataforma e Fluig Identity.
2. **(15 min)** Mapa Mental ao vivo: Desenhar no quadro/tela os 5 pilares e como se conectam.
3. **(20 min)** Arquitetura técnica: servidor JBoss/WildFly, engine Rhino JS, banco de dados, servidor de indexação.

#### Mapa Mental — Os 5 Pilares do Fluig

Desenhar no quadro:

```
                    ┌─────────────┐
                    │   FLUIG     │
                    │ PLATAFORMA  │
                    └──────┬──────┘
           ┌───────┬───────┼───────┬───────┐
           ▼       ▼       ▼       ▼       ▼
      ┌────────┐┌────────┐┌────────┐┌────────┐┌────────┐
      │FORMULÁ-││PROCES- ││DATA-   ││PORTAIS/││WIDGETS │
      │RIOS    ││SOS BPM ││SETS    ││PAGES   ││        │
      │(Forms) ││(BPMN)  ││(Dados) ││(WCM)   ││(Apps)  │
      └────────┘└────────┘└────────┘└────────┘└────────┘
```

**Para cada pilar, perguntar à turma:**
> "Me deem um exemplo do dia a dia de vocês que se encaixa aqui."

Exemplos esperados:
- Formulários → Solicitação de compra, cadastro de fornecedor
- Processos → Aprovação de férias, aprovação de pagamento
- Datasets → Lista de funcionários, consulta de produtos no ERP
- Portais → Intranet da empresa, portal do RH
- Widgets → Dashboard de indicadores, lista de tarefas

#### Arquitetura Técnica — Pontos-Chave

**Servidor de Aplicação (JBoss/WildFly):**
> "O Fluig roda em cima de um JBoss. Isso significa que tudo que vocês desenvolvem — formulários, datasets, eventos — roda no servidor Java. Mas vocês não vão escrever Java. O Fluig tem uma camada intermediária que interpreta JavaScript no servidor."

**Engine Rhino JS — O PONTO MAIS IMPORTANTE:**
> "O Fluig usa o Rhino, que é uma engine JavaScript que roda dentro do Java. Isso significa que o JavaScript dos datasets e eventos de formulário NÃO roda no browser — roda no servidor. Isso tem duas implicações enormes:"

1. **Não tem acesso ao DOM** → Não dá para usar `document.getElementById()` dentro de um dataset ou evento de processo. Isso é JavaScript de servidor.
2. **Não tem ES6+** → Rhino suporta até ES5. Nada de `let`, `const`, `arrow functions`, `template literals`, `async/await`. Só `var`, `function`, concatenação com `+`.

**⚡ Gatilho de discussão (15-20 min):**
> "Por que vocês acham que a TOTVS escolheu Rhino em vez de Node.js? Quais as vantagens e desvantagens?"

Guiar para:
- **Vantagem:** Integração nativa com Java (bibliotecas Java disponíveis direto no JS)
- **Vantagem:** Segurança (sandbox controlado, sem acesso a filesystem)
- **Desvantagem:** Linguagem limitada (ES5), performance inferior ao V8
- **Implicação real:** "Vocês vão escrever JavaScript que parece de 2010. E tudo bem."

---

### ⏱️ [01:10 - 01:20] ☕ INTERVALO (10 min)

---

### ⏱️ [01:20 - 02:05] OS 5 PILARES EM DETALHE (45 min)

**O que fazer:**
Navegar no Fluig ao vivo, mostrando cada pilar na prática. Não codificar — apenas navegar e explicar.

#### Pilar 1: Formulários (10 min)
- Abrir um formulário existente no Fluig
- Mostrar o HTML por trás (inspecionar elemento)
- Explicar: "Todo formulário Fluig é um HTML com convenções específicas. O campo `name` é o que determina a persistência."

**Fala-chave:**
> "Formulário Fluig NÃO é um framework proprietário. É HTML puro com regras. Se você sabe HTML, você já sabe 60% do que precisa."

#### Pilar 2: Processos BPM (10 min)
- Abrir o editor de processos
- Mostrar um workflow simples (início → tarefa → gateway → fim)
- Explicar: atividades, eventos, gateways

**Fala-chave:**
> "O Fluig usa BPMN 2.0, que é um padrão internacional. O que vocês aprenderem aqui vale para qualquer ferramenta BPM do mercado."

#### Pilar 3: Datasets (10 min)
- Abrir a tela de datasets no admin
- Executar um dataset interno (ex: colleague)
- Mostrar o resultado em forma de tabela

**Fala-chave:**
> "Dataset é a forma do Fluig buscar informação. Pode ser do banco interno, de uma API externa, ou de um cálculo customizado. É o coração da plataforma."

#### Pilar 4: Portais/Pages (8 min)
- Navegar por um portal existente
- Mostrar como pages organizam conteúdo
- Explicar WCM (Web Content Management)

#### Pilar 5: Widgets (7 min)
- Mostrar um widget em um portal
- Explicar: "Widget é um componente reutilizável. Vocês podem criar widgets customizados com HTML/CSS/JS."

---

### ⏱️ [02:05 - 02:30] BOAS PRÁTICAS DE PROJETO (25 min)

**O que fazer:**
Apresentar convenções de nomenclatura e organização.

#### Nomenclatura (Slide + discussão)

| Artefato | Convenção | Exemplo |
|----------|-----------|---------|
| Formulário | `frm_[modulo]_[nome]` | `frm_compras_solicitacao` |
| Processo | `wkf_[modulo]_[nome]` | `wkf_compras_aprovacao` |
| Dataset | `ds_[fonte]_[entidade]` | `ds_protheus_fornecedores` |
| Widget | `wdg_[modulo]_[nome]` | `wdg_compras_dashboard` |
| Portal | `prt_[nome]` | `prt_intranet` |

**Fala-chave:**
> "Nomenclatura parece bobagem, mas quando vocês tiverem 200 formulários na plataforma, agradeçam por ter um padrão. O prefixo identifica o tipo, o módulo agrupa por área de negócio."

#### Estrutura de Pastas no Fluig Studio (Slide)

```
projeto-fluig/
├── forms/
│   └── frm_compras_solicitacao/
│       ├── frm_compras_solicitacao.html
│       └── frm_compras_solicitacao.js (eventos)
├── workflow/
│   └── wkf_compras_aprovacao/
│       └── wkf_compras_aprovacao.process
├── datasets/
│   └── ds_protheus_fornecedores.js
├── wcm/
│   └── widgets/
│       └── wdg_compras_dashboard/
└── README.md
```

**⚡ Dica de tempo:** Se estiver adiantado, abrir o Fluig Studio e mostrar essa estrutura ao vivo. Se estiver atrasado, só slide.

---

### ⏱️ [02:30 - 02:40] ☕ INTERVALO (10 min)

---

### ⏱️ [02:40 - 03:10] EXERCÍCIO: NAVEGAR NA PLATAFORMA (30 min)

**O que fazer:**
Exercício guiado — cada aluno loga no Fluig e navega.

**Instruções para a turma:**

> "Agora é com vocês. Quero que naveguem no Fluig e me tragam as respostas para estas perguntas. Podem trabalhar em dupla."

1. Encontre 3 formulários diferentes e anote o nome de cada um
2. Abra um processo e identifique: quantas atividades tem? Tem gateway?
3. Vá até a área de datasets e execute o dataset `colleague`. Quantos registros retornou?
4. Encontre um portal e identifique quais widgets ele usa
5. **Bônus:** Inspecione o HTML de um formulário (F12) e encontre um campo com atributo `name`

**Enquanto a turma faz:** Circular pela sala, ajudar quem tem dificuldade de login ou navegação. Anotar dúvidas recorrentes.

**Revisão (últimos 10 min):** Pedir para 2-3 alunos compartilharem o que encontraram. Discutir.

---

### ⏱️ [03:10 - 03:35] MATERIAIS DE ESTUDO E LINKS ÚTEIS (25 min)

**O que fazer:**
Tour pela documentação oficial. Abrir cada link e navegar brevemente.

#### Links Essenciais

| Recurso | URL | Para que serve |
|---------|-----|----------------|
| TDN - Fluig | `tdn.totvs.com/display/public/fluig` | Documentação oficial completa |
| Fluig Style Guide | `style.fluig.com` | Componentes visuais, padrões CSS |
| Fluig Dev Guide | `tdn.totvs.com/display/public/fluig/Dev+Guide` | Guia do desenvolvedor |
| API REST Fluig | `tdn.totvs.com/display/public/fluig/REST+API` | Referência da API |
| Fluig SDK | `tdn.totvs.com/display/public/fluig/SDK` | Classes Java/JS disponíveis |
| Forum TOTVS | `forum.fluig.com` | Comunidade, perguntas e respostas |

**Fala-chave:**
> "Esses links vão ser os melhores amigos de vocês. A TDN é a bíblia do Fluig. O Style Guide é o catálogo de componentes visuais. E o fórum é onde vocês resolvem os problemas que a documentação não cobre."

**⚡ Dica de tempo (buffer):** Se estiver adiantado, fazer um exercício rápido: "Encontrem na TDN a documentação de um evento de formulário chamado displayFields. Quem achar primeiro?"

---

### ⏱️ [03:35 - 03:50] DISCUSSÃO: EXPECTATIVAS E AUTOMAÇÃO (15 min)

**O que fazer:**
Discussão aberta com a turma.

> "Agora que vocês viram a plataforma por dentro, me contem: que processo da empresa de vocês gostariam de automatizar no Fluig? Pode ser algo que hoje é feito no papel, por email, ou numa planilha."

**Por que isso funciona:**
- Engaja a turma ao conectar o conteúdo com a realidade deles
- Dá a você, instrutor, uma noção do que eles precisam (pode ajustar exemplos futuros)
- Gasta tempo de forma produtiva e natural

**Anotar respostas** — vão servir como exemplos nas próximas sessões.

---

### ⏱️ [03:50 - 04:00] ENCERRAMENTO (10 min)

**3 Takeaways do dia:**
1. "O Fluig tem 5 pilares que se conectam: Forms, BPM, Datasets, Portais e Widgets"
2. "O JavaScript do Fluig roda no SERVIDOR com Rhino (ES5) — não é o mesmo JS do browser"
3. "Nomenclatura e organização definem se seu projeto vai escalar ou virar um caos"

**Preview da próxima sessão:**
> "Na quinta, vamos colocar a mão na massa pela primeira vez. Vamos configurar o Eclipse, instalar o plugin do Fluig Studio, fazer nosso primeiro deploy, e começar a explorar o Style Guide. Tragam o notebook configurado — vou mandar por email os pré-requisitos de instalação."

**Enviar por email após a aula:**
- Link para download do Eclipse
- Link para o plugin do Fluig Studio
- Versão do Java necessária
- Credenciais de acesso ao servidor Fluig

---

## NOTAS DO INSTRUTOR

### Se sobrar tempo (15+ min)
- Aprofundar na discussão Rhino vs Node.js
- Fazer um quiz rápido: "O que é um dataset? a) tabela do banco b) função que retorna dados c) componente visual"
- Explorar mais o Fluig Studio (mostrar a interface sem configurar)

### Se faltar tempo
- Comprimir "Materiais de estudo" para 10 min (só mostrar a TDN e o Style Guide)
- Pular a discussão final e ir direto para os takeaways
- O exercício de navegação pode ser reduzido para 20 min

### Riscos da sessão
- **Aluno sem acesso ao Fluig** → Ter um acesso genérico de backup
- **Ambiente fora do ar** → Usar screenshots nos slides como fallback
- **Turma muito heterogênea** → Observar quem são os devs e quem são os analistas para formar duplas complementares nas próximas sessões