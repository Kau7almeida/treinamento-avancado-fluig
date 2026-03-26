# 📋 COLA RÁPIDA — Sessão 04: Formulários Avançados e Técnicas Dinâmicas

---

## Zoom — Busca avançada com modal

```javascript
// Configuração do Zoom
var opts = {
    source: 'ds_fornecedores',          // Dataset fonte
    displayKey: 'cod_fornecedor',        // Campo exibido no input
    formFieldsList: [                    // Mapeamento dataset → formulário
        { textFieldDatasetName: 'codigo', textFieldFormName: 'cod_fornecedor' },
        { textFieldDatasetName: 'nome',   textFieldFormName: 'nome_fornecedor' }
    ],
    tableHeader: ['Código', 'Nome'],     // Cabeçalhos da modal
    tableFieldsList: ['codigo', 'nome'], // Campos exibidos na modal
    maxResult: 10                        // Paginação
};

FLUIGC.zoom('#cod_fornecedor', opts);

// Eventos
$('#cod_fornecedor').on('fluig.zoom.itemSelect', function(e, item) { });
$('#cod_fornecedor').on('fluig.zoom.itemRemove', function(e) { });
```

---

## AutoComplete — Sugestões inline

```javascript
var opts = {
    source: 'ds_produtos',
    displayKey: 'descricao',
    minLength: 2,
    maxResult: 10
};

FLUIGC.autocomplete('#produto', opts);

$('#produto').on('fluig.autocomplete.itemSelect', function(e, item) { });
```

---

## Pai × Filho — Cascateamento de selects

```javascript
// REGRA: sempre limpar o filho antes de popular
$('#estado').change(function() {
    var uf = $(this).val();
    var $cidade = $('#cidade');

    $cidade.empty();  // OBRIGATÓRIO: limpar

    if (!uf) {
        $cidade.append('<option value="">Selecione o estado primeiro...</option>');
        $cidade.prop('disabled', true);
        return;
    }

    // Buscar via dataset com constraint
    var constraints = [];
    constraints.push(
        DatasetFactory.createConstraint("uf", uf, uf, ConstraintType.MUST)
    );
    var dados = DatasetFactory.getDataset("ds_cidades", null, constraints, null);

    $cidade.append('<option value="">Selecione...</option>');
    for (var i = 0; i < dados.values.length; i++) {
        $cidade.append(
            '<option value="' + dados.getValue(i, "codigo") + '">' +
            dados.getValue(i, "nome") + '</option>'
        );
    }
    $cidade.prop('disabled', false);
});
```

---

## Select com Dataset — Combos dinâmicos

```javascript
$(document).ready(function() {
    var constraints = [];
    var dados = DatasetFactory.getDataset("ds_departamentos", null, constraints, null);
    var $select = $('#departamento');
    $select.empty();
    $select.append('<option value="">Selecione...</option>');
    for (var i = 0; i < dados.values.length; i++) {
        $select.append(
            '<option value="' + dados.getValue(i, "codigo") + '">' +
            dados.getValue(i, "nome") + '</option>'
        );
    }
});
```

---

## Lista Dinâmica — Adicionar/remover linhas

### Convenção dos 3 underscores

```
name="item_descricao___1"   → linha 1, coluna descricao
name="item_descricao___2"   → linha 2, coluna descricao
name="item_quantidade___1"  → linha 1, coluna quantidade
```

O Fluig cria a tabela filha automaticamente no banco.

### Adicionar linha

```javascript
var contador = 1;
$('#btn-adicionar').click(function() {
    contador++;
    var nova = '<tr class="linha-item" data-linha="' + contador + '">' +
        '<td>' + contador + '</td>' +
        '<td><input type="text" class="form-control input-sm" ' +
        'name="item_descricao___' + contador + '"></td>' +
        // ... demais campos com ___contador
        '</tr>';
    $('#corpo-tabela').append(nova);
});
```

### Remover linha

```javascript
$(document).on('click', '.btn-remover', function() {
    if ($('.linha-item').length <= 1) {
        alert('Mínimo 1 item'); return;
    }
    $(this).closest('tr').remove();
    reindexarLinhas();  // OBRIGATÓRIO após remover
});
```

### Reindexar (OBRIGATÓRIO após remover)

```javascript
function reindexarLinhas() {
    $('.linha-item').each(function(index) {
        var num = index + 1;
        $(this).find('.num-linha').text(num);
        $(this).find('input[name^="item_descricao___"]')
               .attr('name', 'item_descricao___' + num);
        // ... repetir para cada campo
    });
}
```

---

## Quando usar o quê?

| Cenário | Componente |
|---------|-----------|
| 2-4 opções fixas (Sim/Não, prioridade) | Radio |
| 5-50 opções (departamentos) | Select |
| 5-50 opções dinâmicas | Select com Dataset |
| 50+ opções (fornecedores, produtos) | Zoom |
| Busca rápida com sugestão | AutoComplete |
| Campos dependentes (Estado→Cidade) | Pai × Filho |
| Múltiplos registros (itens do pedido) | Lista Dinâmica |

---

## Troubleshooting Sessão 04

| Problema | Causa | Solução |
|----------|-------|---------|
| Zoom abre modal vazia | Dataset não existe ou nome errado | Verificar nome exato (case sensitive) |
| Zoom não preenche campos | Mapeamento errado em formFieldsList | Conferir textFieldDatasetName vs textFieldFormName |
| Pai×Filho: cidade antiga persiste | Não limpou o campo filho | Adicionar `$cidade.empty()` antes de popular |
| Select mostra "Carregando..." | Dataset não carregou | Verificar console (F12) por erros |
| Lista perde dados ao salvar | Names sem ___ ou índice errado | Verificar padrão `nome___N` |
| Lista após remover: dados trocados | Não reindexou | Chamar `reindexarLinhas()` após remover |
| Cálculo retorna NaN | Formato de moeda (1.234,56) | Converter: `str.replace(/\./g,'').replace(',','.')` |

---

*Sessão 04 — Treinamento Avançado Fluig | JYNX*