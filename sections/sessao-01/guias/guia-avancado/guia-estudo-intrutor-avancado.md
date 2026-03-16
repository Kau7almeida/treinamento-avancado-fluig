# 🔬 Guia de Estudo Avançado do Instrutor — Sessão 01
## Arquitetura Profunda, Código Real e Debugging
### Treinamento Avançado Fluig | JYNX

> **Este material complementa o `guia-estudo-instrutor.md` básico.**
> Leia o básico primeiro (45 min), depois este avançado (60-90 min).
> Aqui não tem resumo — tem profundidade. É o material que transforma um instrutor bom em um tech lead na frente da turma.

---

## 1. ARQUITETURA TÉCNICA EM PROFUNDIDADE

### 1.1 O Servidor de Aplicação: JBoss → WildFly

O Fluig roda dentro do **WildFly**, que é a versão open-source do JBoss Application Server. A TOTVS migrou do JBoss 6 para o WildFly 10 por volta de 2016, e com isso também migrou do Java 6 para o Java 8.

**O que o WildFly faz pelo Fluig:**

- **Gerenciamento de conexões HTTP:** Todas as requisições dos browsers dos usuários chegam ao WildFly, que roteia para os módulos corretos do Fluig
- **Pool de conexões com banco:** O WildFly mantém um pool de conexões JDBC abertas. Quando um dataset precisa consultar o banco, não abre uma conexão nova — pega uma do pool. Isso é crítico para performance
- **Gerenciamento de transações:** Quando um formulário é salvo, o WildFly garante que todas as operações (salvar dados, mover processo, enviar notificação) aconteçam atomicamente — ou todas passam ou todas falham
- **EJBs (Enterprise Java Beans):** Os serviços internos do Fluig são implementados como EJBs. Quando o Rhino chama `hAPI.moveTo()`, por baixo está invocando um EJB que gerencia o ciclo de vida do processo
- **Classloader isolado:** Cada módulo do Fluig tem seu próprio classloader. Isso significa que um bug em um dataset não derruba o servidor inteiro — o classloader isola o problema

**Arquivo de configuração principal:** `standalone.xml`

Este arquivo fica em `WILDFLY_HOME/standalone/configuration/` e controla tudo: datasources (conexão com banco), subsistema de logging, pool de threads, configurações de segurança. O instrutor não precisa editar, mas saber que existe impressiona a turma técnica.

```xml
<!-- Exemplo simplificado do standalone.xml -->
<subsystem xmlns="urn:jboss:domain:datasources:4.0">
    <datasource jndi-name="java:/jdbc/FluigDS" pool-name="FluigDS">
        <connection-url>jdbc:postgresql://localhost:5432/fluig</connection-url>
        <driver>postgresql</driver>
        <pool>
            <min-pool-size>10</min-pool-size>
            <max-pool-size>100</max-pool-size>
        </pool>
    </datasource>
</subsystem>
```

**Por que isso importa para o instrutor:** Se alguém perguntar "por que o Fluig tá lento?", a resposta pode estar no pool de conexões do `standalone.xml` — se o `max-pool-size` for muito baixo para o número de usuários, as requisições ficam na fila.

### 1.2 Ciclo de Vida de um Deploy

Quando o instrutor faz "Export → fluig" no Eclipse, o que acontece por baixo:

```
1. Eclipse/Studio empacota os artefatos em um arquivo .war ou .ear
2. O pacote é enviado via HTTP para a API de deploy do Fluig
3. O Fluig recebe e descompacta no diretório de artefatos
4. O classloader carrega as novas classes/scripts
5. Formulários HTML são registrados no ECM
6. Processos BPMN são registrados no BPM engine
7. Datasets JS são registrados no registry de datasets
8. O Solr reindexa os novos conteúdos
9. O Fluig invalida caches relacionados aos artefatos atualizados
```

**Problema comum:** O passo 9 (invalidação de cache) às vezes falha silenciosamente. É por isso que um "Ctrl+F5" no browser resolve muitos problemas pós-deploy — o browser estava usando a versão cacheada do formulário.

**Outro problema:** O passo 4 (classloader) pode falhar se houver conflito de versões de bibliotecas. Se o instrutor incluir um JAR que conflita com uma biblioteca interna do Fluig, o deploy passa mas o artefato falha em runtime.

### 1.3 Como o Fluig Persiste Dados de Formulários

Quando um formulário é **cadastrado** no Fluig (não é o deploy do código, é o registro do formulário como tipo de documento):

1. O Fluig lê o HTML do formulário
2. Identifica todos os campos com atributo `name`
3. Cria uma **tabela no banco** com uma coluna para cada campo `name`
4. A tabela é nomeada automaticamente (geralmente com um ID numérico)
5. Campos de **tabela pai** (fora de qualquer `<table>`) viram colunas da tabela principal
6. Campos dentro de tabelas HTML com padrão `tablename___` viram uma **tabela filha** no banco (relação 1:N)

**Exemplo prático:**

```html
<!-- Campos da tabela pai (1 registro por formulário) -->
<input name="solicitante" type="text">
<input name="data_solicitacao" type="date">
<input name="departamento" type="text">

<!-- Campos da tabela filha (N registros por formulário) -->
<table>
  <tr>
    <td><input name="item_desc___1" type="text"></td>
    <td><input name="item_qtd___1" type="number"></td>
    <td><input name="item_valor___1" type="text"></td>
  </tr>
</table>
```

**No banco, isso gera:**

```
Tabela principal (1 registro):
| documentid | solicitante    | data_solicitacao | departamento |
|------------|----------------|------------------|--------------|
| 12345      | João Silva     | 2026-03-17       | Compras      |

Tabela filha "tablename" (N registros):
| documentid | item_desc        | item_qtd | item_valor |
|------------|------------------|----------|------------|
| 12345      | Notebook Dell    | 2        | 4500.00    |
| 12345      | Mouse Wireless   | 5        | 89.90      |
```

**Convenção dos 3 underscores `___`:** É assim que o Fluig identifica que campos pertencem a uma tabela filha. O sufixo `___1`, `___2` etc. indica a linha. Isso vai ser crucial nas sessões 03 e 04 quando trabalharmos com listas dinâmicas.

### 1.4 Arquitetura de Módulos Internos

```
┌─────────────────────────────────────────────────────────────┐
│                    WildFly (JVM)                            │
│                                                             │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────────┐ │
│  │   ECM    │  │   BPM    │  │   WCM    │  │ Integration│ │
│  │          │  │          │  │          │  │            │ │
│  │ - Forms  │  │ - Engine │  │ - Pages  │  │ - Datasets │ │
│  │ - Docs   │  │ - BPMN   │  │ - Portals│  │ - REST API │ │
│  │ - GED    │  │ - Events │  │ - Widgets│  │ - SOAP     │ │
│  │ - Versão │  │ - hAPI   │  │ - Layout │  │ - ESB      │ │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └─────┬──────┘ │
│       │             │             │               │        │
│  ┌────▼─────────────▼─────────────▼───────────────▼──────┐ │
│  │              Rhino JS Engine (ES5)                     │ │
│  │   → Executa datasets, eventos, callbacks               │ │
│  │   → Ponte entre JS do desenvolvedor e Java interno     │ │
│  └───────────────────────┬───────────────────────────────┘ │
│                          │                                  │
│  ┌───────────────────────▼───────────────────────────────┐ │
│  │              Java SDK (hAPI, fluigAPI, etc)            │ │
│  └───────────────────────┬───────────────────────────────┘ │
│                          │                                  │
│  ┌───────────────────────▼──────┐  ┌──────────────────────┐│
│  │     Banco de Dados (JDBC)    │  │   Solr (Indexação)   ││
│  │  PostgreSQL/MySQL/Oracle/SS  │  │   Full-text search   ││
│  └──────────────────────────────┘  └──────────────────────┘│
└─────────────────────────────────────────────────────────────┘
```

**Fluxo de uma requisição típica:**

1. Usuário clica "Salvar" no formulário (browser)
2. Browser envia POST HTTP para o WildFly
3. WildFly roteia para o módulo ECM
4. ECM invoca o evento `beforeTaskSave` via Rhino
5. Rhino executa o JS do desenvolvedor
6. Se o JS chama `DatasetFactory.getDataset()`, o Rhino invoca o Java SDK
7. O Java SDK acessa o banco via JDBC (pool de conexões do WildFly)
8. O resultado volta: Java → Rhino → ECM → persiste dados → BPM move processo → HTTP response → browser atualiza

---

## 2. RHINO JS — CÓDIGO REAL E WORKAROUNDS

### 2.1 Limitações Reais com Exemplos

**Limitação 1: Sem `let`/`const` — e o problema do escopo**

No ES5, `var` tem escopo de função, não de bloco. Isso causa bugs sutis:

```javascript
// PROBLEMA: var vaza do for
function processarItens(items) {
    for (var i = 0; i < items.length; i++) {
        var item = items[i];  // CUIDADO: var é reatribuída a cada iteração
        // mas se tiver um callback assíncrono aqui,
        // todos vão referenciar o ÚLTIMO valor de item
    }
    // i e item ainda existem aqui! var vaza do for.
    log.info("Último i: " + i);  // funciona! não é undefined
}

// WORKAROUND: usar IIFE para criar escopo
for (var i = 0; i < items.length; i++) {
    (function(index) {
        // index é uma cópia local, segura
        var item = items[index];
    })(i);
}
```

**Limitação 2: Sem template literals — concatenação segura**

```javascript
// ES6 (PROIBIDO no Rhino):
// var msg = `Olá ${nome}, seu pedido #${numero} foi aprovado`;

// ES5 (CORRETO):
var msg = "Olá " + nome + ", seu pedido #" + numero + " foi aprovado";

// WORKAROUND para strings longas (mais legível):
var msg = [
    "Olá ", nome, ",",
    " seu pedido #", numero,
    " foi aprovado em ", data,
    " pelo aprovador ", aprovador
].join("");
```

**Limitação 3: Sem `Array.from`, `map`, `filter` nativos em alguns contextos**

```javascript
// ES6 (PROIBIDO):
// var nomes = usuarios.map(u => u.nome);
// var ativos = usuarios.filter(u => u.ativo);

// ES5 (CORRETO):
var nomes = [];
for (var i = 0; i < usuarios.length; i++) {
    nomes.push(usuarios[i].nome);
}

var ativos = [];
for (var i = 0; i < usuarios.length; i++) {
    if (usuarios[i].ativo) {
        ativos.push(usuarios[i]);
    }
}

// WORKAROUND: criar funções utilitárias reutilizáveis
function mapArray(arr, fn) {
    var result = [];
    for (var i = 0; i < arr.length; i++) {
        result.push(fn(arr[i], i));
    }
    return result;
}

function filterArray(arr, fn) {
    var result = [];
    for (var i = 0; i < arr.length; i++) {
        if (fn(arr[i], i)) {
            result.push(arr[i]);
        }
    }
    return result;
}

// Uso (quase tão limpo quanto ES6):
var nomes = mapArray(usuarios, function(u) { return u.nome; });
var ativos = filterArray(usuarios, function(u) { return u.ativo; });
```

**Limitação 4: Sem `JSON.parse` confiável em todas as versões**

```javascript
// Em versões mais antigas do Fluig, JSON pode não estar disponível
// WORKAROUND: verificar antes de usar
if (typeof JSON === "undefined") {
    // fallback manual ou usar classe Java
    var jsonString = new java.lang.String(inputStr);
    // parse manual...
}

// Na prática, versões recentes do Fluig têm JSON.
// Mas sempre envolva em try/catch:
try {
    var dados = JSON.parse(responseBody);
} catch (e) {
    log.error("Erro ao parsear JSON: " + e.message);
    log.error("Response era: " + responseBody);
    // tratar o erro graciosamente
}
```

### 2.2 Usando Classes Java no Rhino — Exemplos Reais

**Chamada REST usando `java.net.URL`:**

```javascript
function chamarApiRest(urlStr, metodo, authHeader) {
    var url = new java.net.URL(urlStr);
    var conn = url.openConnection();

    conn.setRequestMethod(metodo || "GET");
    conn.setRequestProperty("Content-Type", "application/json");
    conn.setRequestProperty("Accept", "application/json");

    // Basic Auth
    if (authHeader) {
        conn.setRequestProperty("Authorization", authHeader);
    }

    // Timeout (em milissegundos)
    conn.setConnectTimeout(10000);  // 10 segundos para conectar
    conn.setReadTimeout(30000);     // 30 segundos para ler resposta

    var responseCode = conn.getResponseCode();

    if (responseCode >= 200 && responseCode < 300) {
        var inputStream = conn.getInputStream();
        var reader = new java.io.BufferedReader(
            new java.io.InputStreamReader(inputStream, "UTF-8")
        );

        var response = "";
        var line;
        while ((line = reader.readLine()) !== null) {
            response += line;
        }
        reader.close();

        return {
            status: responseCode,
            body: response,
            success: true
        };
    } else {
        return {
            status: responseCode,
            body: null,
            success: false,
            error: "HTTP " + responseCode
        };
    }
}

// Uso:
var resultado = chamarApiRest(
    "https://protheus.empresa.com/rest/SA2",
    "GET",
    "Basic " + java.util.Base64.getEncoder().encodeToString(
        new java.lang.String("usuario:senha").getBytes("UTF-8")
    )
);
```

**Usando `java.util.HashMap` para estruturas complexas:**

```javascript
// Quando precisa passar parâmetros para serviços Java
var params = new java.util.HashMap();
params.put("userId", "admin");
params.put("companyId", 1);
params.put("documentId", 12345);

// O HashMap é aceito nativamente por métodos do SDK
var result = fluigAPI.getDocumentService().getDocumentById(params);
```

**Logging no servidor (sem console.log):**

```javascript
// console.log NÃO EXISTE no Rhino!
// Use o logger do Fluig:
var log = com.datasul.technology.webdesk.logging.LogManager.getLogger("meu-dataset");

log.info("Iniciando processamento");
log.warn("Valor inesperado: " + valor);
log.error("Erro na integração: " + e.message);

// O log aparece no arquivo de log do WildFly:
// WILDFLY_HOME/standalone/log/server.log
```

### 2.3 O Ciclo de Vida dos Eventos de Formulário

Essa é a ordem EXATA em que os eventos disparam quando um usuário interage com um formulário em um processo:

```
Usuário ABRE a tarefa:
  1. displayFields(form, customHTML)     → Controla visibilidade dos campos
  2. enableFields(form, customHTML)      → Controla se campos são editáveis
  3. inputFields(form, customHTML)       → Define valores iniciais

Usuário CLICA em "Enviar":
  4. validateForm(form)                  → Valida dados (throw para bloquear!)
  5. beforeTaskSave(colleagueId, visaId, movementSequence)
                                         → Lógica antes de salvar
  6. [FLUIG SALVA NO BANCO]
  7. afterTaskSave(colleagueId, visaId, movementSequence)
                                         → Lógica depois de salvar

  8. beforeTaskComplete(colleagueId, visaId, movementSequence)
                                         → Lógica antes de completar tarefa
  9. [FLUIG MOVE O PROCESSO PARA PRÓXIMA ETAPA]
  10. afterTaskComplete(colleagueId, visaId, movementSequence)
                                          → Lógica depois de completar
```

**Regras de ouro:**
- `displayFields` → Esconder campos por etapa (ex: campo "aprovador" só aparece na etapa 2)
- `enableFields` → Desabilitar campos (ex: "data" é readonly após preenchimento)
- `validateForm` → Usar `throw "Mensagem"` para impedir envio. **NÃO use `return false`** — não funciona!
- `beforeTaskSave` → Lógica leve, síncrona. Se demorar, o formulário "trava"
- `afterTaskComplete` → Melhor lugar para lógica pesada (integração, email) porque o dado já está salvo

---

## 3. OS 5 PILARES COM CÓDIGO REAL

### 3.1 ECM — Formulário real completo

```html
<!-- frm_compras_solicitacao.html -->
<div class="fluig-style-guide" style="padding: 20px;">

    <div class="panel panel-default">
        <div class="panel-heading">
            <h3 class="panel-title">Solicitação de Compras</h3>
        </div>
        <div class="panel-body">
            <div class="row">
                <div class="col-md-6">
                    <div class="form-group">
                        <label for="solicitante">Solicitante</label>
                        <input type="text" class="form-control"
                               id="solicitante" name="solicitante"
                               readonly>
                    </div>
                </div>
                <div class="col-md-3">
                    <div class="form-group">
                        <label for="data_solicitacao">Data</label>
                        <input type="date" class="form-control"
                               id="data_solicitacao" name="data_solicitacao">
                    </div>
                </div>
                <div class="col-md-3">
                    <div class="form-group">
                        <label for="centro_custo">Centro de Custo</label>
                        <select class="form-control"
                                id="centro_custo" name="centro_custo">
                            <option value="">Selecione...</option>
                        </select>
                    </div>
                </div>
            </div>
        </div>
    </div>

</div>

<script>
// ESTE CÓDIGO RODA NO BROWSER (ES6 OK!)
$(document).ready(function() {
    // Preencher nome do solicitante automaticamente
    var currentUser = WCMAPI.getUser();
    $('#solicitante').val(currentUser);

    // Preencher data com hoje
    var hoje = new Date().toISOString().split('T')[0];
    $('#data_solicitacao').val(hoje);
});
</script>
```

### 3.2 BPM — Evento de processo

```javascript
// Este código roda no SERVIDOR (Rhino ES5)
// Evento: afterTaskComplete

function afterTaskComplete(colleagueId, visaId, movementSequence) {
    // Pegar dados do formulário via hAPI
    var valorTotal = parseFloat(hAPI.getCardValue("valor_total"));

    // Regra de negócio: alçada de aprovação
    if (valorTotal > 10000) {
        // Mover para atividade "Aprovação Diretoria"
        hAPI.setAutomaticDecision(3, [], "Valor acima de R$10.000");
    } else {
        // Mover para atividade "Aprovação Gerente"
        hAPI.setAutomaticDecision(2, [], "Valor dentro da alçada");
    }
}
```

### 3.3 Dataset Customizado

```javascript
// datasets/ds_departamentos.js
// Este código roda no SERVIDOR (Rhino ES5)

function createDataset(fields, constraints, sortFields) {
    // 1. Criar estrutura do dataset
    var dataset = DatasetBuilder.newDataset();
    dataset.addColumn("codigo");
    dataset.addColumn("nome");
    dataset.addColumn("gerente");

    // 2. Adicionar dados
    dataset.addRow(["TI", "Tecnologia da Informação", "João Silva"]);
    dataset.addRow(["RH", "Recursos Humanos", "Maria Santos"]);
    dataset.addRow(["FIN", "Financeiro", "Carlos Lima"]);
    dataset.addRow(["COM", "Compras", "Ana Costa"]);

    // 3. Retornar (OBRIGATÓRIO! Esquecer isso = dataset retorna null)
    return dataset;
}
```

**Consumindo o dataset no formulário (browser):**

```javascript
// ESTE CÓDIGO RODA NO BROWSER
$(document).ready(function() {
    var c1 = DatasetFactory.createConstraint("codigo", "", "", ConstraintType.MUST);
    var dados = DatasetFactory.getDataset("ds_departamentos", null, [c1], null);

    for (var i = 0; i < dados.values.length; i++) {
        var codigo = dados.getValue(i, "codigo");
        var nome = dados.getValue(i, "nome");
        $('#centro_custo').append(
            '<option value="' + codigo + '">' + nome + '</option>'
        );
    }
});
```

**Consumindo o dataset dentro de um evento (servidor):**

```javascript
// ESTE CÓDIGO RODA NO SERVIDOR (Rhino ES5)
function beforeTaskSave(colleagueId, visaId, movementSequence) {
    var constraints = [];
    constraints.push(
        DatasetFactory.createConstraint("codigo", "TI", "TI", ConstraintType.MUST)
    );

    var dataset = DatasetFactory.getDataset("ds_departamentos", null, constraints, null);

    if (dataset.rowsCount > 0) {
        var gerente = dataset.getValue(0, "gerente");
        hAPI.setCardValue("aprovador", gerente);
    }
}
```

### 3.4 WCM — Widget básico

```javascript
// wcm/widget/wdg_compras_dashboard/wdg_compras_dashboard.js

var WdgComprasDashboard = SuperWidget.extend({

    // Inicialização do widget
    init: function() {
        // Carregar dados ao iniciar
        this.carregarDados();
    },

    // Binding de eventos (ações do usuário)
    bindings: {
        local: {
            'click .btn-atualizar': ['carregarDados']
        }
    },

    // Métodos
    carregarDados: function() {
        var self = this;
        var constraints = [];

        DatasetFactory.getDataset(
            "ds_solicitacoes_pendentes",
            null,
            constraints,
            null,
            {
                success: function(data) {
                    self.renderizarTabela(data);
                },
                error: function(jqXHR, textStatus, errorThrown) {
                    self.showError("Erro ao carregar dados: " + textStatus);
                }
            }
        );
    },

    renderizarTabela: function(data) {
        var html = '<table class="table table-striped">';
        html += '<thead><tr><th>ID</th><th>Solicitante</th><th>Valor</th></tr></thead>';
        html += '<tbody>';

        for (var i = 0; i < data.values.length; i++) {
            html += '<tr>';
            html += '<td>' + data.getValue(i, "documentid") + '</td>';
            html += '<td>' + data.getValue(i, "solicitante") + '</td>';
            html += '<td>R$ ' + data.getValue(i, "valor_total") + '</td>';
            html += '</tr>';
        }

        html += '</tbody></table>';
        this.$el.find('.dashboard-content').html(html);
    }
});
```

---

## 4. CENÁRIOS DE DEBUGGING

### 4.1 "O formulário salvou mas o campo está vazio"

**Sintomas:** Usuário preenche o campo, clica salvar, abre o registro — campo está em branco.

**Diagnóstico passo a passo:**

```
Passo 1: Inspecionar o HTML (F12 no browser)
  → O campo tem atributo "name"?
  → Se NÃO → adicionar name. Problema resolvido.
  → Se SIM → ir para passo 2.

Passo 2: O name tem caracteres especiais?
  → Espaços, acentos, hífens, pontos?
  → Se SIM → trocar para snake_case sem acentos
  → Se NÃO → ir para passo 3.

Passo 3: O campo está dentro da tag <form>?
  → Fluig usa um <form> wrapper. Campo fora dele não é enviado.
  → Verificar se não há </form> prematura fechando antes do campo.

Passo 4: Existe outro campo com o mesmo name?
  → Procurar no HTML: Ctrl+F pelo valor do name
  → Se duplicado → o segundo sobrescreve o primeiro

Passo 5: Algum evento JS está limpando o campo?
  → Verificar displayFields, enableFields
  → Um displayFields que esconde o campo pode limpar o valor
  → Solução: usar enableFields para desabilitar em vez de esconder
```

### 4.2 "O dataset retorna null ou vazio"

**Diagnóstico:**

```
Passo 1: O dataset está registrado?
  → Admin → Datasets → Buscar pelo nome (case sensitive!)
  → Se não aparece → deploy não funcionou. Refazer deploy.

Passo 2: O dataset tem "return dataset;" no final?
  → Erro mais comum. A função createDataset PRECISA retornar o dataset.
  → Abrir o código e verificar a última linha.

Passo 3: Tem erro de sintaxe ES6?
  → let, const, arrow function, template literal?
  → Rhino não mostra erro claro — apenas retorna null
  → Solução: revisar todo o código procurando ES6

Passo 4: O dataset chama uma API externa?
  → A API está acessível do servidor? (não do browser!)
  → Testar com: curl [URL] a partir do servidor
  → Verificar firewall entre servidor Fluig e API externa

Passo 5: O constraint está correto?
  → Constraint com campo que não existe no dataset → retorna vazio
  → Constraint com valor errado → retorna vazio sem erro
  → Testar sem constraints primeiro para isolar o problema
```

### 4.3 "O processo trava — não avança para a próxima etapa"

**Diagnóstico:**

```
Passo 1: Verificar o Painel de Controle
  → Admin → Processos → Monitorar → Buscar a solicitação
  → Em qual atividade está parado?
  → Tem algum erro registrado?

Passo 2: Verificar condições do Gateway
  → Se o processo passa por um gateway exclusivo:
    → TODAS as saídas precisam ter condição definida
    → Se nenhuma condição é atendida → processo trava
    → Verificar o valor do campo que controla o gateway

Passo 3: Verificar destinatário da próxima atividade
  → A atividade seguinte tem destinatário definido?
  → Mecanismo de atribuição está configurado?
  → O usuário destinatário existe e está ativo?

Passo 4: Verificar eventos
  → O beforeTaskComplete tem erro? → Processo não completa
  → O afterTaskComplete tem hAPI.setAutomaticDecision com ID errado?
  → Verificar log do servidor para exceções Java

Passo 5: Log do servidor
  → WILDFLY_HOME/standalone/log/server.log
  → Buscar pelo ID do documento ou pelo nome do processo
  → Erros de Rhino aparecem como ScriptException no log
```

### 4.4 "O Style Guide não está funcionando"

**Diagnóstico:**

```
Passo 1: O formulário tem o wrapper correto?
  → Precisa ter: <div class="fluig-style-guide">
  → Sem isso, os componentes não recebem os estilos

Passo 2: O formulário está sendo aberto dentro do Fluig?
  → Se abrir o HTML direto no browser (file://), não tem Style Guide
  → Precisa abrir dentro da plataforma Fluig

Passo 3: CSS customizado está conflitando?
  → Um style="..." inline pode sobrescrever o Style Guide
  → Uma tag <style> no formulário pode interferir
  → Solução: usar apenas classes do Style Guide, evitar CSS inline

Passo 4: Versão do Fluig suporta o componente?
  → Componentes mais novos do Style Guide podem não existir
    em versões antigas do Fluig
  → Verificar a versão da plataforma e a documentação do componente
```

---

## 5. DICAS AVANÇADAS PARA O INSTRUTOR

### 5.1 Se alguém perguntar sobre performance

"O Fluig fica lento quando tem muitos usuários. O que fazer?"

**Resposta arquitetural:**
- Verificar `max-pool-size` no `standalone.xml` (pool de conexões)
- Datasets sem constraint retornando milhares de registros → adicionar filtros
- Datasets jornalizados com intervalo de atualização muito curto → aumentar intervalo
- Solr desatualizado → re-indexar (admin → System → Re-index)
- JVM com memória insuficiente → aumentar `-Xmx` no `standalone.conf`

### 5.2 Se alguém perguntar sobre segurança

"O que impede um usuário de ver dados que não deveria?"

**Resposta:**
- **Nível de formulário:** Permissões ECM controlam quem pode ver/editar cada documento
- **Nível de processo:** Cada atividade define quem é o destinatário — só essa pessoa vê a tarefa
- **Nível de dataset:** Datasets podem verificar o usuário logado e filtrar dados
- **Nível de portal:** Widgets podem ser restritos por papel/grupo
- **CUIDADO:** `displayFields` esconde o campo visualmente, mas o dado AINDA ESTÁ no HTML. Para segurança real, use permissões no backend, não esconda no frontend

### 5.3 Se alguém perguntar sobre versionamento

"Como faço controle de versão do código Fluig?"

**Resposta:**
- O Fluig **não tem** versionamento de código nativo (não é como Git)
- A recomendação é usar Git externamente (como estamos fazendo no repositório!)
- O fluig Studio exporta os artefatos como arquivos — versione esses arquivos no Git
- Para documentos ECM, o Fluig tem versionamento de documento (1.0, 2.0, etc) — mas é para conteúdo, não para código

---

*Guia de Estudo Avançado do Instrutor — Sessão 01*
*Treinamento Avançado Fluig | JYNX*
*Tempo de leitura estimado: 60-90 minutos*