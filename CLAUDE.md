## Sobre o Repositório

Repositório operacional do **Treinamento Avançado Fluig (48h)** da JYNX, estruturado em 12 sessões de 4h cada. O projeto integrador é um **Sistema de Solicitação de Compras** que evolui da sessão 05 a 12, integrando formulários, workflows BPM, datasets com consulta REST ao Protheus, portais e widgets.

## Estrutura do Repositório

```
sections/sessao-XX/           ← Uma pasta por sessão (01 a 12)
  ├── roteiro-instrutor.md    ← Roteiro detalhado com timeline, falas e estratégias
  ├── README.md               ← Cola rápida da sessão (troubleshooting, referências)
  ├── slides.pptx             ← Apresentação (identidade visual JYNX)
  └── codigos/                ← Códigos prontos para live coding e exercícios
docs/roteiro-macro/           ← Roteiro macro com ementa, cronograma e detalhamento de todas as sessões
datasets-protheus/            ← Datasets e mocks JSON para integração REST com Protheus
project-integration/          ← Código do projeto integrador por milestone
```

**Nota:** O README.md raiz documenta a pasta `sessoes/`, mas a pasta real é `sections/`.

## Padrões Técnicos Obrigatórios

### JavaScript Server-side (Rhino ES5)
Datasets, eventos de formulário e eventos de processo rodam no servidor Fluig via **engine Rhino (ES5)**. Ao gerar este tipo de código:
- Usar apenas `var` (nunca `let`/`const`)
- Usar apenas `function(){}` (nunca arrow functions)
- Concatenar strings com `+` (nunca template literals)
- Sem `Promise`, `async/await`, `Array.from()`, spread operator
- Classes Java são acessíveis: `java.util.HashMap`, `java.net.URL`, etc.

### JavaScript Front-end (Browser)
Scripts dentro de `<script>` em formulários e widgets rodam no browser e aceitam ES6+. Priorizar:
- jQuery no padrão Fluig
- Componentes do fluig Style Guide (https://style.fluig.com)
- Namespace ou IIFE para evitar escopo global

### Formulários HTML
- Obrigatório: atributo `name` em todo campo que precisa persistir dados
- Wrapper `<div class="fluig-style-guide">` no elemento raiz
- Classes CSS do Style Guide (Bootstrap-like): `form-control`, `form-group`, `panel`, `col-md-*`
- Grid de 12 colunas: `col-md-*` (desktop), `col-sm-*` (tablet), `col-xs-*` (mobile)

## Convenções de Nomenclatura JYNX

| Artefato    | Prefixo | Padrão                     | Exemplo                       |
|-------------|---------|----------------------------|-------------------------------|
| Formulario  | `frm_`  | `frm_[modulo]_[nome]`     | `frm_compras_solicitacao`     |
| Processo    | `wkf_`  | `wkf_[modulo]_[nome]`     | `wkf_compras_aprovacao`       |
| Dataset     | `ds_`   | `ds_[fonte]_[entidade]`   | `ds_protheus_fornecedores`    |
| Widget      | `wdg_`  | `wdg_[modulo]_[nome]`     | `wdg_compras_dashboard`       |
| Portal      | `prt_`  | `prt_[nome]`              | `prt_intranet`                |

Arquivos Markdown: `roteiro-instrutor.md`, `README.md`. Nomes de pastas em kebab-case: `sessao-01`, `milestone-01-formulario-base`.

## Integração REST com Protheus

Escopo limitado a **consultas GET** (Fluig consulta, nunca grava no ERP). Autenticação via Basic Auth.

| Dataset Fluig         | Endpoint Protheus           | Uso no Projeto                      |
|-----------------------|-----------------------------|-------------------------------------|
| `ds_fornecedores`     | SA2 (Fornecedores)         | Zoom no formulário                  |
| `ds_produtos`         | SB1 (Produtos)             | Zoom na lista dinamica              |
| `ds_centros_custo`    | CTT (Centros de Custo)     | Select no formulario                |
| `ds_cond_pagamento`   | SE4 (Cond. Pagamento)      | Select no formulario                |

Datasets de integração devem ter tratamento de erro (timeout, 401, 500) e mocks JSON como fallback.

## Eventos de Formulario Fluig

Cada evento tem um proposito especifico. Referencia rapida:
- `displayFields` / `enableFields` — controle de visibilidade/habilitacao por etapa
- `validateForm` — bloqueia envio (usa `throw` para impedir, nao `return false`)
- `beforeTaskSave` / `afterTaskSave` — logica antes/apos salvar
- `beforeTaskComplete` / `afterTaskComplete` — logica antes/apos completar tarefa
- SDK: `hAPI` (mover processo), `fluigAPI`, `DatasetFactory` (consultar datasets em eventos)

## Regras Operacionais

1. Cada sessao deve ser autocontida (material completo na propria pasta)
2. Materiais reutilizaveis (datasets, mocks) ficam em pastas centralizadas, sem duplicacao
3. Slides seguem identidade visual JYNX (laranja #E85D04, Trebuchet MS)
4. O projeto integrador acompanha a evolucao real da trilha (milestones progressivos)

## Blocos Tematicos das Sessoes

| Sessoes | Bloco           | Conteudo Principal                                  |
|---------|-----------------|-----------------------------------------------------|
| 01-02   | Fundacao        | Arquitetura, ambiente Eclipse, Style Guide          |
| 03-04   | Formularios     | Forms customizados, Zoom, Pai x Filho, listas       |
| 05, 08  | Projeto         | Milestone do projeto integrador (hands-on)          |
| 06-07   | Workflows       | BPMN 2.0, gateways, eventos de formulario, SDK      |
| 09-10   | Dados           | Datasets customizados, integracao REST Protheus      |
| 11-12   | Portais         | Pages, WCM, widgets, graficos, projeto final         |
