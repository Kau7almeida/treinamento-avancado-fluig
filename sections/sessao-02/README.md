# 📋 COLA RÁPIDA — Sessão 02: Configuração do Ambiente e fluig Style Guide

---

## Configuração do Eclipse + fluig Studio

### Pré-requisitos
| Item | Versão | Verificação |
|------|--------|-------------|
| Java JDK | 8 ou 11 | `java -version` no terminal |
| Eclipse | IDE for Java EE | Help → About Eclipse IDE |
| RAM mínima | 4 GB | |
| Espaço em disco | 2 GB livres | |

### Instalação do Plugin fluig Studio

```
1. Eclipse → Help → Install New Software
2. Add → Name: "fluig Studio"
         URL:  http://fluig.totvs.com/update-site/
3. Selecionar todos os itens → Next → Accept → Finish
4. Reiniciar Eclipse
5. Window → Perspective → Open Perspective → fluig
```

### Configuração do Servidor

```
1. Window → Preferences → fluig → Servers
2. Add Server:
   - Nome: servidor-treinamento
   - Host: [URL DO SERVIDOR]
   - Porta: [PORTA]
   - Login: [USUÁRIO]
   - Senha: [SENHA]
3. Test Connection → "Connected successfully"
```

### Primeiro Deploy

```
1. Botão direito no projeto → Export → fluig
2. Selecionar servidor
3. Marcar artefatos para deploy
4. Finish
5. Verificar no browser: Fluig → Documentos → Novo → Formulário
```

---

## Estrutura de Projeto no fluig Studio

```
meu-projeto/
├── forms/                    ← Formulários HTML
│   └── frm_hello_fluig/
│       └── frm_hello_fluig.html
├── workflow/                 ← Processos BPMN (.process)
├── datasets/                 ← Datasets customizados (.js)
├── wcm/                      ← Portais e Widgets
│   ├── widget/               ← Widgets customizados
│   └── layout/               ← Layouts de portal
├── events/                   ← Eventos globais
└── resources/                ← Imagens, CSS, JS compartilhados
```

**Regra de ouro:** O Studio identifica o tipo de artefato pela pasta. Formulário fora de `forms/` não faz deploy como formulário.

---

## fluig Style Guide — Componentes Essenciais

### Grid System (Layout Responsivo)
```html
<!-- Linha com 2 colunas iguais -->
<div class="row">
    <div class="col-md-6">Coluna esquerda</div>
    <div class="col-md-6">Coluna direita</div>
</div>

<!-- Linha com 3 colunas -->
<div class="row">
    <div class="col-md-4">1/3</div>
    <div class="col-md-4">1/3</div>
    <div class="col-md-4">1/3</div>
</div>

<!-- Responsivo: 2 colunas no desktop, 1 no mobile -->
<div class="row">
    <div class="col-md-6 col-xs-12">Adaptável</div>
    <div class="col-md-6 col-xs-12">Adaptável</div>
</div>
```

**Regra:** Colunas somam 12 por linha. `col-md-*` = desktop, `col-sm-*` = tablet, `col-xs-*` = mobile.

### Inputs
```html
<!-- Text input com label -->
<div class="form-group">
    <label for="nome">Nome</label>
    <input type="text" class="form-control" id="nome" name="nome"
           placeholder="Digite o nome">
</div>

<!-- Select -->
<div class="form-group">
    <label for="departamento">Departamento</label>
    <select class="form-control" id="departamento" name="departamento">
        <option value="">Selecione...</option>
        <option value="TI">TI</option>
        <option value="RH">RH</option>
        <option value="Financeiro">Financeiro</option>
    </select>
</div>

<!-- Textarea -->
<div class="form-group">
    <label for="observacao">Observação</label>
    <textarea class="form-control" id="observacao" name="observacao"
              rows="3"></textarea>
</div>

<!-- Checkbox -->
<div class="checkbox">
    <label>
        <input type="checkbox" name="urgente" value="S"> Solicitação urgente
    </label>
</div>

<!-- Radio -->
<div class="radio">
    <label>
        <input type="radio" name="prioridade" value="alta"> Alta
    </label>
</div>
<div class="radio">
    <label>
        <input type="radio" name="prioridade" value="normal"> Normal
    </label>
</div>
```

### Botões
```html
<button type="button" class="btn btn-primary">Salvar</button>
<button type="button" class="btn btn-default">Cancelar</button>
<button type="button" class="btn btn-danger">Excluir</button>
<button type="button" class="btn btn-link">Ver detalhes</button>

<!-- Tamanhos -->
<button class="btn btn-primary btn-lg">Grande</button>
<button class="btn btn-primary btn-sm">Pequeno</button>
<button class="btn btn-primary btn-xs">Mini</button>
```

### Painéis (Agrupamento)
```html
<div class="panel panel-default">
    <div class="panel-heading">
        <h3 class="panel-title">Dados do Solicitante</h3>
    </div>
    <div class="panel-body">
        <!-- Campos aqui dentro -->
    </div>
</div>
```

### Tabelas
```html
<table class="table table-striped table-hover">
    <thead>
        <tr>
            <th>Código</th>
            <th>Descrição</th>
            <th>Valor</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>001</td>
            <td>Notebook Dell</td>
            <td>R$ 4.500,00</td>
        </tr>
    </tbody>
</table>
```

### Alertas
```html
<div class="alert alert-success">Formulário salvo com sucesso!</div>
<div class="alert alert-warning">Atenção: campos obrigatórios.</div>
<div class="alert alert-danger">Erro ao salvar o formulário.</div>
<div class="alert alert-info">Dica: use Tab para navegar entre campos.</div>
```

### Modal
```html
<!-- Botão que abre o modal -->
<button class="btn btn-primary" data-toggle="modal"
        data-target="#meuModal">Abrir Modal</button>

<!-- Modal -->
<div class="modal fade" id="meuModal">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close"
                        data-dismiss="modal">&times;</button>
                <h4 class="modal-title">Confirmação</h4>
            </div>
            <div class="modal-body">
                <p>Deseja realmente excluir?</p>
            </div>
            <div class="modal-footer">
                <button class="btn btn-default"
                        data-dismiss="modal">Cancelar</button>
                <button class="btn btn-danger">Excluir</button>
            </div>
        </div>
    </div>
</div>
```

---

## Atalhos Úteis do Eclipse

| Atalho | Ação |
|--------|------|
| `Ctrl + S` | Salvar arquivo |
| `Ctrl + Shift + F` | Formatar código (indentação) |
| `Ctrl + D` | Deletar linha |
| `Ctrl + /` | Comentar/descomentar linha |
| `Ctrl + Space` | Autocomplete |
| `Ctrl + Shift + R` | Buscar arquivo por nome |
| `Alt + Shift + R` | Renomear |
| `F5` | Refresh do projeto |

---

## Troubleshooting Sessão 02

| Problema | Causa Provável | Solução |
|----------|---------------|---------|
| `java -version` não encontrado | Java não instalado ou JAVA_HOME não configurado | Instalar JDK 8/11 e configurar variáveis de ambiente |
| Plugin não aparece após install | Eclipse não reiniciou ou URL errada | Reiniciar Eclipse; verificar URL do update site |
| "Connection refused" ao conectar | URL/porta errada ou firewall | Verificar URL (http vs https), porta, e regras de firewall |
| Deploy falha sem erro | Artefato não marcado para export | Verificar se marcou os itens na tela de export |
| Deploy falha com erro de permissão | Usuário sem role de admin/dev | Solicitar permissão ao admin do Fluig |
| Formulário não aparece no Fluig | Deploy feito mas cache não atualizou | Limpar cache do browser (Ctrl+Shift+Del) ou Ctrl+F5 |
| Eclipse muito lento | Memória insuficiente | Editar eclipse.ini: `-Xmx1024m` → `-Xmx2048m` |
| Style Guide não carrega | site fora ou bloqueado | Usar versão offline dos componentes |

---

## Links desta sessão

| Recurso | URL |
|---------|-----|
| Eclipse Download | https://www.eclipse.org/downloads/ |
| fluig Studio Update Site | http://fluig.totvs.com/update-site/ |
| fluig Style Guide | https://style.fluig.com |
| Documentação Style Guide | https://tdn.totvs.com/display/public/fluig/Style+Guide |

---

*Sessão 02 — Treinamento Avançado Fluig | JYNX*