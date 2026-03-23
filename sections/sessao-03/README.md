# 📋 COLA RÁPIDA — Sessão 03: Formulários Customizados e Dinâmicos

---

## Estrutura Obrigatória de um Formulário Fluig

```html
<div class="fluig-style-guide" style="padding: 20px;">
    <div class="panel panel-default">
        <div class="panel-heading">
            <h3 class="panel-title">Título do Painel</h3>
        </div>
        <div class="panel-body">
            <div class="row">
                <div class="col-md-6">
                    <div class="form-group">
                        <label for="campo">Label</label>
                        <input type="text" class="form-control"
                               id="campo" name="campo">
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

**Hierarquia:** `fluig-style-guide` → `panel` → `row` → `col-md-X` → `form-group` → `label` + `input`

---

## Regras do Atributo name

| Regra | Correto | Errado |
|-------|---------|--------|
| Snake case | `nome_completo` | `nomeCompleto` |
| Sem acentos | `descricao` | `descrição` |
| Sem espaços | `data_nascimento` | `data nascimento` |
| Sem hífens | `centro_custo` | `centro-custo` |
| Sem duplicatas | Um `name` por campo | Dois campos com mesmo `name` |

**Campo sem `name` = dado perdido.**
**Campo `disabled` = dado NÃO enviado. Use `readonly` para preservar o dado.**

---

## Tipos de Input — Referência Rápida

| Tipo | HTML | Persiste como |
|------|------|---------------|
| Texto | `<input type="text" name="x">` | String |
| Email | `<input type="email" name="x">` | String (com validação) |
| Número | `<input type="number" name="x">` | String (converter no servidor) |
| Data | `<input type="date" name="x">` | String (YYYY-MM-DD) |
| Select | `<select name="x"><option value="v">` | O `value` da option selecionada |
| Radio | `<input type="radio" name="x" value="v">` | O `value` do selecionado |
| Checkbox | `<input type="checkbox" name="x" value="S">` | Só envia se marcado |
| Textarea | `<textarea name="x">` | String multilinha |
| Hidden | `<input type="hidden" name="x" value="v">` | String (invisível ao usuário) |

---

## Helpers do Fluig

```javascript
// Usuário logado
WCMAPI.getUser()           // login do usuário
WCMAPI.getUserCode()       // código numérico

// Modo do formulário
document.getElementById("___FORM_MODE___")
// "ADD" = novo | "MOD" = editando | "VIEW" = visualizando

// Número do documento
document.getElementById("___NUM_DOCUMENT___")

// Data de hoje
var hoje = new Date().toISOString().split('T')[0];
```

---

## Validação em 3 Camadas

```javascript
// CAMADA 1: HTML nativo (grátis, sem JS)
<input type="text" name="nome" required>
<input type="email" name="email">
<input type="number" name="qtd" min="1" max="100">

// CAMADA 2: jQuery no browser
$('#btn-salvar').click(function() {
    if ($('#nome').val().trim() === '') {
        alert('Nome é obrigatório!');
        $('#nome').focus();
        return false;
    }
});

// CAMADA 3: validateForm no servidor (sessão 07)
// function validateForm(form) {
//     if (form.getValue("valor") > 10000) {
//         throw "Valor acima do permitido!";
//     }
// }
```

---

## Máscaras com jQuery Mask

```javascript
// Incluir jquery.mask.min.js no resources/ do projeto
$('#cpf').mask('000.000.000-00');
$('#cnpj').mask('00.000.000/0000-00');
$('#telefone').mask('(00) 00000-0000');
$('#cep').mask('00000-000');
$('#valor').mask('000.000.000,00', {reverse: true});
```

---

## Troubleshooting Sessão 03

| Problema | Causa | Solução |
|----------|-------|---------|
| Campo não salva | Falta `name` | Adicionar `name="campo"` |
| Campo salva vazio | `name` com acento/espaço | Usar snake_case sem acentos |
| Estilo não aplica | Falta wrapper | Adicionar `<div class="fluig-style-guide">` |
| Select salva texto em vez de código | `value` errado na `<option>` | Verificar `value` de cada option |
| Radio não funciona como grupo | `name` diferente | Todos os radio do grupo com mesmo `name` |
| Checkbox não salva quando desmarcado | Comportamento padrão HTML | Tratar no servidor (campo ausente = não marcado) |
| Campo disabled não salva | `disabled` não envia dados | Usar `readonly` em vez de `disabled` |
| Máscara não funciona | jQuery Mask não carregado | Incluir jquery.mask.min.js no resources/ |
| Bootstrap conflitando | CDN importado manualmente | Nunca importar Bootstrap — Style Guide já inclui |

---

## Convenção Visual Padrão

```
fluig-style-guide
└── panel panel-default
    ├── panel-heading
    │   └── panel-title
    └── panel-body
        └── row
            ├── col-md-6 (metade)
            │   └── form-group
            │       ├── label
            │       └── input.form-control
            └── col-md-6 (metade)
                └── form-group
                    ├── label
                    └── input.form-control
```

---

*Sessão 03 — Treinamento Avançado Fluig | JYNX*