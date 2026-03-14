# 📋 COLA RÁPIDA — Sessão 01: Introdução, Ferramentas e Boas Práticas

---

## Os 5 Pilares do Fluig

| Pilar | O que é | Exemplo |
|-------|---------|---------|
| **Formulários** | Interface HTML para entrada de dados | Solicitação de compra |
| **Processos (BPM)** | Workflow BPMN 2.0 para automatizar fluxos | Aprovação de pagamento |
| **Datasets** | Funções que retornam dados (internos ou externos) | Lista de fornecedores do Protheus |
| **Portais/Pages** | Páginas web gerenciáveis (WCM) | Intranet corporativa |
| **Widgets** | Componentes reutilizáveis em portais | Dashboard de indicadores |

---

## Arquitetura Técnica — Resumo

```
┌─────────────────────────────────────────┐
│            BROWSER (Cliente)            │
│  HTML + CSS + JS (ES6 ok aqui)          │
│  jQuery, Style Guide, DOM manipulation  │
└─────────────────┬───────────────────────┘
                  │ HTTP
┌─────────────────▼───────────────────────┐
│       SERVIDOR FLUIG (JBoss/WildFly)    │
│                                         │
│  ┌─────────────┐  ┌──────────────────┐  │
│  │  Rhino JS   │  │   Java SDK       │  │
│  │  (ES5 only) │  │   (hAPI, etc)    │  │
│  │             │  │                  │  │
│  │ - Datasets  │  │ - Serviços REST  │  │
│  │ - Eventos   │  │ - Indexação      │  │
│  │ - Callbacks │  │ - Segurança      │  │
│  └─────────────┘  └──────────────────┘  │
│                                         │
│  ┌──────────────────────────────────┐   │
│  │         Banco de Dados           │   │
│  │   (dados de forms, processos)    │   │
│  └──────────────────────────────────┘   │
└─────────────────────────────────────────┘
```

---

## JavaScript no Fluig — Onde roda o quê?

| Contexto | Onde roda | Engine | Versão JS | Acesso ao DOM? |
|----------|-----------|--------|-----------|----------------|
| Script no formulário (`<script>`) | **Browser** | V8/SpiderMonkey | ES6+ | ✅ Sim |
| Eventos de formulário (displayFields, etc) | **Servidor** | Rhino | ES5 | ❌ Não |
| Datasets customizados | **Servidor** | Rhino | ES5 | ❌ Não |
| Eventos de processo (beforeTaskSave, etc) | **Servidor** | Rhino | ES5 | ❌ Não |
| JavaScript em Widget | **Browser** | V8/SpiderMonkey | ES6+ | ✅ Sim |

### O que NÃO pode usar no Rhino (ES5):
```javascript
// ❌ PROIBIDO no servidor (datasets, eventos)
let x = 1;              // usar: var x = 1;
const y = 2;             // usar: var y = 2;
() => {}                 // usar: function() {}
`template ${var}`        // usar: "texto " + var
async/await              // usar: callbacks síncronos
Array.from()             // usar: loop manual
[...spread]              // usar: Array.prototype.slice.call()
```

### O que PODE usar no Rhino:
```javascript
// ✅ FUNCIONA no servidor
var x = 1;
function minhaFuncao() { }
for (var i = 0; i < arr.length; i++) { }
JSON.parse() / JSON.stringify()
try/catch
// Classes Java via integração:
var HashMap = java.util.HashMap;
```

---

## Nomenclatura Padrão JYNX

| Artefato | Prefixo | Padrão | Exemplo |
|----------|---------|--------|---------|
| Formulário | `frm_` | `frm_[modulo]_[nome]` | `frm_compras_solicitacao` |
| Processo | `wkf_` | `wkf_[modulo]_[nome]` | `wkf_compras_aprovacao` |
| Dataset | `ds_` | `ds_[fonte]_[entidade]` | `ds_protheus_fornecedores` |
| Widget | `wdg_` | `wdg_[modulo]_[nome]` | `wdg_compras_dashboard` |
| Portal | `prt_` | `prt_[nome]` | `prt_intranet` |

---

## Links Úteis

| Recurso | URL |
|---------|-----|
| TDN Fluig (Documentação) | https://tdn.totvs.com/display/public/fluig |
| Fluig Style Guide | https://style.fluig.com |
| Dev Guide | https://tdn.totvs.com/display/public/fluig/Dev+Guide |
| API REST | https://tdn.totvs.com/display/public/fluig/REST+API |
| SDK Reference | https://tdn.totvs.com/display/public/fluig/SDK |
| Fórum TOTVS | https://forum.fluig.com |

---

## Troubleshooting Sessão 01

| Problema | Causa provável | Solução |
|----------|---------------|---------|
| Não consigo logar no Fluig | Credenciais erradas ou expiradas | Pedir reset ao admin |
| Fluig muito lento | Muitos usuários simultâneos | Verificar com TI se servidor suporta a turma |
| Não encontro a área de datasets | Permissão insuficiente | Solicitar perfil de admin/desenvolvedor |
| F12 não mostra HTML do formulário | Form renderizado em iframe | Navegar dentro do iframe no DevTools |

---

## Conceitos-Chave Para Lembrar

1. **Formulário Fluig = HTML + convenções.** O atributo `name` dos campos é o que persiste os dados.
2. **JavaScript de servidor (Rhino) ≠ JavaScript do browser.** Não tente usar ES6 em datasets/eventos.
3. **Dataset não é tabela do banco.** É uma função que retorna dados em formato tabular.
4. **BPMN 2.0 é padrão internacional.** O conhecimento vale fora do Fluig.
5. **Nomenclatura e organização salvam projetos.** Defina o padrão ANTES de criar artefatos.

---

*Sessão 01 — Treinamento Avançado Fluig | JYNX*