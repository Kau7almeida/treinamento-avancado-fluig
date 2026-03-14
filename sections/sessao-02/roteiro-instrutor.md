# SESSÃO 02 — Configuração do Ambiente e fluig Style Guide
## Roteiro do Instrutor | 20/03/2026 (Quinta) | 4 horas

---

## OBJETIVO DA SESSÃO
Ambiente de desenvolvimento configurado, primeiro deploy realizado com sucesso ("Hello Fluig"), e domínio dos padrões visuais do Style Guide. Ao final, o aluno consegue criar, deploiar e visualizar um formulário simples no Fluig.

---

## PREPARAÇÃO (Antes da aula)

### Checklist
- [ ] Confirmar versão do Java instalada nas máquinas dos alunos (Java 8 ou 11)
- [ ] URL de download do Eclipse testada e funcional
- [ ] URL do update site do plugin fluig Studio anotada
- [ ] Credenciais do servidor Fluig prontas (uma por aluno ou genérica)
- [ ] Formulário "Hello Fluig" pré-pronto como backup (se deploy falhar)
- [ ] Site do Style Guide (style.fluig.com) aberto e testado
- [ ] Slides da sessão 02 abertos

### Material de Apoio
- Slides: `sessao-02/slides.pptx`
- Códigos: `sessao-02/codigos/`
- Cola: `sessao-02/README.md`

### ⚠️ ALERTA: Sessão de Alto Risco
Esta é a sessão com maior probabilidade de problemas técnicos. Ambiente, Java, plugin, firewall, permissões — tudo pode dar errado. Estratégia: reserve mais tempo do que acha necessário para troubleshooting. Se um aluno travar, peça para outro ajudar (pair troubleshooting). Ter o formulário "Hello Fluig" pré-pronto permite avançar mesmo se alguém não conseguir configurar.

---

## TIMELINE DETALHADA

---

### ⏱️ [00:00 - 00:15] RECONEXÃO (15 min)

**O que fazer:**
1. Pedir para um aluno resumir a sessão 01 (5 pilares, Rhino, boas práticas)
2. Complementar pontos que ficaram de fora
3. Conectar com o tema do dia: "Vocês viram a teoria, agora vamos colocar a mão na massa"

**Fala sugerida:**
> "Quem topa resumir o que vimos na segunda? Pode ser por tópicos mesmo."

Após o resumo:
> "Perfeito. Hoje é o dia mais técnico do treinamento inteiro — vamos configurar o ambiente, fazer nosso primeiro deploy, e conhecer o Style Guide. É normal ter problemas de configuração, então paciência: o importante é que todo mundo saia daqui com o Eclipse rodando."

---

### ⏱️ [00:15 - 01:00] CONFIGURAÇÃO DO ECLIPSE + PLUGIN (45 min)

**O que fazer:**
Guiar a turma passo a passo na instalação e configuração.

#### Passo 1: Eclipse IDE (10 min)
> "Quem já tem o Eclipse instalado? Ótimo. Quem não tem, vamos instalar agora."

- Abrir o site do Eclipse e baixar a versão **Eclipse IDE for Java EE Developers**
- Se já tiverem instalado: verificar a versão (Help → About)

**Fala-chave:**
> "O Fluig Studio é um plugin do Eclipse. Então o Eclipse é o 'carro' e o plugin é o 'GPS'. Sem o carro, o GPS não funciona."

#### Passo 2: Plugin fluig Studio (20 min)
Guiar a instalação do plugin:

1. Eclipse → Help → Install New Software
2. Adicionar URL do update site: `http://fluig.totvs.com/update-site/`
3. Selecionar todos os componentes do fluig Studio
4. Next → Accept → Finish → Restart

**Erro proposital (se quiser):**
> Digitar a URL errada propositalmente (`http://fluig.totvs.com/update-sit/` sem o 'e') → Mostrar o erro → "Viram? Um caractere errado e nada funciona. Atenção aos detalhes."

#### Passo 3: Verificação (15 min)
Após restart do Eclipse:
1. Verificar se a perspectiva "fluig" aparece (Window → Perspective → Open Perspective)
2. Criar um novo projeto fluig (File → New → fluig Project)
3. Verificar estrutura de pastas gerada

**⚡ Troubleshooting ativo:**
Enquanto a turma faz a instalação, circular pela sala. Problemas comuns:
- Java não encontrado → Verificar JAVA_HOME
- Plugin não aparece → Reiniciar Eclipse, verificar URL
- Eclipse muito lento → Aumentar memória no eclipse.ini (-Xmx1024m)

---

### ⏱️ [01:00 - 01:15] TROUBLESHOOTING DE AMBIENTE (15 min)

**O que fazer:**
Buffer dedicado para resolver problemas. Se não houver problemas (improvável), avançar.

**Se todos configuraram com sucesso:**
> Usar esse tempo para explorar a interface do fluig Studio — menus, atalhos, perspectivas.

**Se alguém ainda tem problemas:**
> Pedir para um colega que já configurou sentar ao lado e ajudar. "Pair troubleshooting" — o aluno que ajuda reforça o aprendizado.

---

### ⏱️ [01:15 - 01:25] ☕ INTERVALO (10 min)

---

### ⏱️ [01:25 - 01:55] ESTRUTURA DE PROJETOS NO FLUIG STUDIO (30 min)

**O que fazer:**
Criar o primeiro projeto e explicar cada pasta.

#### Criando o projeto (10 min)
1. File → New → fluig Project
2. Nome do projeto: `treinamento-avancado`
3. Mostrar a estrutura gerada

#### Anatomia do projeto (20 min)

Navegar pela estrutura e explicar cada pasta:

```
treinamento-avancado/
├── forms/                    ← Formulários HTML
│   └── (vazio por enquanto)
├── workflow/                 ← Processos BPMN
│   └── (vazio por enquanto)
├── datasets/                 ← Datasets customizados
│   └── (vazio por enquanto)
├── wcm/                      ← Portais e Widgets
│   ├── widget/
│   └── layout/
├── events/                   ← Eventos globais
└── resources/                ← Recursos (imagens, CSS, JS)
```

**Fala-chave:**
> "Cada pasta tem um propósito. Forms guarda formulários, workflow guarda processos, datasets guarda — adivinhem — datasets. É a mesma nomenclatura que aprendemos na sessão 01. Organização desde o dia zero."

**Comparativo arquitetural:**
> "Por que o Fluig obriga essa estrutura de pastas? Porque o deploy é automatizado — o Studio sabe que tudo dentro de forms/ é formulário e faz o deploy correto. Se você colocar um formulário na pasta errada, o deploy ignora."

---

### ⏱️ [01:55 - 02:30] CONEXÃO COM SERVIDOR + PRIMEIRO DEPLOY (35 min)

**O que fazer:**
Conectar ao servidor Fluig e fazer o primeiro deploy do formulário "Hello Fluig".

#### Conectando ao servidor (10 min)

1. No fluig Studio: Window → Preferences → fluig → Servers
2. Add Server:
   - Nome: `servidor-treinamento`
   - Host: `[URL do servidor do cliente]`
   - Porta: `[porta]`
   - Usuário: `[credencial]`
   - Senha: `[credencial]`
3. Test Connection → "Connected successfully"

**Erro proposital:**
> Digitar a URL com http em vez de https (ou vice-versa) → Erro de conexão → "Viram? Protocolo errado. Sempre verifiquem http vs https."

#### Criando o formulário Hello Fluig (10 min)

**Live coding narrado:**

> "Agora vamos criar nosso primeiro formulário. É simples, mas é o marco zero do nosso treinamento."

1. Botão direito em `forms/` → New → Form
2. Nome: `frm_hello_fluig`
3. O Studio gera o HTML básico
4. Editar o HTML:

```html
<!-- Mostrar e narrar cada linha -->
```

(Código completo está em `sessao-02/codigos/frm_hello_fluig.html`)

#### Deploy e teste (15 min)

1. Botão direito no projeto → Export → fluig
2. Selecionar o servidor configurado
3. Deploy
4. Abrir no navegador → Fluig → Documentos → Novo → Formulário

**MOMENTO DE CELEBRAÇÃO:**
> "Parabéns! Vocês acabaram de fazer o primeiro deploy. Isso que vocês estão vendo no browser foi criado no Eclipse, empacotado pelo Studio, enviado para o servidor, e renderizado pelo Fluig. A partir de agora, tudo que construirmos segue esse mesmo fluxo."

**Se o deploy falhar:**
- Verificar console do Eclipse (tab Problems/Console)
- Verificar credenciais do servidor
- **Backup:** Abrir o formulário pré-deployado e seguir a demonstração

---

### ⏱️ [02:30 - 02:40] ☕ INTERVALO (10 min)

---

### ⏱️ [02:40 - 03:20] TOUR PELO FLUIG STYLE GUIDE (40 min)

**O que fazer:**
Navegar pelo Style Guide como um catálogo de componentes. Abrir `style.fluig.com` na tela.

#### Por que Style Guide? (5 min)

**Fala-chave:**
> "O Style Guide é o sistema de design do Fluig. Ele garante que todos os formulários e portais tenham a mesma cara. É como um catálogo IKEA de componentes — você escolhe o que precisa, copia o código, e usa."

**Arquitetural:**
> "Por que usar o Style Guide em vez de CSS próprio? Três razões: consistência visual, manutenção simplificada, e compatibilidade garantida com atualizações do Fluig. Se você fizer CSS customizado que conflita com o Style Guide, uma atualização da plataforma pode quebrar tudo."

#### Navegação guiada (20 min)

Percorrer os componentes mais usados:

1. **Grid System** (col-md-X) — layout responsivo
   > "O grid é a fundação. Tudo se organiza em 12 colunas."

2. **Inputs** — text, select, textarea, checkbox, radio
   > "Notem como cada input já vem com estilos, estados (focus, error, disabled), e classes CSS padronizadas."

3. **Botões** — primary, default, danger, link
   > "Botão primário para ação principal, default para secundária, danger para ações destrutivas."

4. **Tabelas** — table, table-striped, table-hover
   > "Tabela com hover é obrigatório em formulários com lista de itens."

5. **Alertas e Modais** — alert-success, alert-danger, modal
   > "Alerts para feedback inline, modais para confirmações."

6. **Painéis** — panel, panel-heading, panel-body
   > "Painéis são perfeitos para agrupar campos no formulário."

#### Exercício interativo (15 min)
> "Agora quero que vocês façam uma caça ao tesouro no Style Guide. Encontrem:"

1. O componente de **breadcrumb** — onde está? Como usa?
2. O componente de **pagination** — quantos estilos tem?
3. O componente de **tooltip** — como ativa via JavaScript?
4. **Bônus:** Encontrem o componente `fluig-autocomplete` — ele vai ser muito importante na sessão 04.

---

### ⏱️ [03:20 - 03:50] EXERCÍCIO: FORMULÁRIO COM STYLE GUIDE (30 min)

**O que fazer:**
Cada aluno cria um formulário usando pelo menos 5 componentes do Style Guide.

**Instruções:**

> "Agora vocês vão criar um formulário simples — pode ser sobre qualquer tema (cadastro de cliente, pedido de material, registro de chamado). A única regra: usem pelo menos 5 componentes diferentes do Style Guide. Grid, inputs, botão, painel, e mais um à escolha."

**Template base** disponível em `sessao-02/codigos/frm_template_style_guide.html`

**Enquanto a turma faz:**
- Circular pela sala
- Ajudar quem travar
- Observar quem avança rápido (candidatos a desafio extra)
- Tirar dúvidas de CSS/layout

**Revisão (últimos 10 min):**
- Pedir para 2-3 alunos mostrarem seu formulário
- Discutir escolhas: "Por que usou panel aqui? Boa escolha. E se tivesse usado grid em vez de tabela?"

---

### ⏱️ [03:50 - 04:00] ENCERRAMENTO (10 min)

**3 Takeaways do dia:**
1. "O fluig Studio é plugin do Eclipse — projetos têm estrutura de pastas obrigatória"
2. "Deploy = Export para o servidor. Testou, exportou, verificou no browser"
3. "Style Guide é o catálogo de componentes — use-o sempre, nunca reinvente a roda"

**Preview da próxima sessão:**
> "Na segunda vamos construir formulários de verdade. Cada tipo de input, validações, e como o Fluig persiste os dados. Tragam ideias de formulários que gostariam de criar — vamos ter exercícios práticos bem intensos."

---

## NOTAS DO INSTRUTOR

### Se sobrar tempo (15+ min)
- Explorar mais componentes do Style Guide (gráficos, wizard, progress bar)
- Fazer um formulário coletivo: cada aluno adiciona um campo
- Mostrar a documentação de um componente completo (ex: fluig-switch)
- Começar a falar de eventos de formulário como preview da sessão 03

### Se faltar tempo
- Comprimir o tour pelo Style Guide (mostrar só grid, inputs e botões)
- O exercício do formulário pode virar "tarefa de casa"
- Pular a caça ao tesouro no Style Guide

### Riscos da sessão
- **Java incompatível** → Ter um instalador Java 8/11 no pendrive
- **Eclipse não abre** → Verificar memória RAM (mínimo 4GB)
- **Plugin não instala** → Ter o plugin como ZIP offline para install local
- **Servidor indisponível** → Ter formulários pré-deployados como backup
- **Style Guide fora do ar** → Ter screenshots dos componentes nos slides