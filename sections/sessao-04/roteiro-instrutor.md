# SESSÃO 04 — Formulários Avançados e Técnicas Dinâmicas
## Roteiro do Instrutor | 27/03/2026 (Quinta) | 4 horas

---

## OBJETIVO DA SESSÃO
Formulários avançados com carregamento dinâmico de dados e relacionamento entre campos. Ao final, o aluno domina Zoom, Select com Dataset, técnica Pai × Filho, e Listas Dinâmicas — os 4 recursos mais usados em formulários corporativos.

---

## PREPARAÇÃO (Antes da aula)

### Checklist
- [ ] Dataset de exemplo `ds_departamentos` deployado no ambiente (criado na sessão 03 ou deployar agora)
- [ ] Dataset de estados/cidades para demo Pai × Filho (ou usar o código de mock)
- [ ] Formulários de exemplo da pasta `codigos/` testados no ambiente
- [ ] jQuery Mask plugin disponível no resources/ do projeto (se não tiver, funciona sem)

### Material de Apoio
- Slides: `sessao-04/slides.pptx`
- Códigos: `sessao-04/codigos/`
- Cola: `sessao-04/README.md`
- Mini site alunos: `sessao-04/index.html`

---

## TIMELINE DETALHADA

---

### ⏱️ [00:00 - 00:15] RECONEXÃO (15 min)

**Fala sugerida:**
> "Quem resume a sessão passada? Qual a estrutura obrigatória de um formulário Fluig? E o que acontece se um campo não tiver name?"

Após o resumo:
> "Hoje vamos dar um salto. Os formulários que vocês fizeram na segunda eram estáticos — os dados das options estavam fixos no HTML. Hoje vamos tornar tudo dinâmico: combos que carregam dados do servidor, campos que se relacionam entre si, e tabelas onde o usuário adiciona e remove linhas."

---

### ⏱️ [00:15 - 00:45] CAMPO ZOOM (30 min)

**O que é o Zoom:**
> "Zoom é um campo de busca avançada do Fluig. O usuário digita parte do texto, uma janela modal abre com resultados filtrados, ele seleciona e os dados preenchem campos do formulário automaticamente. É como um autocomplete turbinado com modal de pesquisa."

**Demonstração primeiro (5 min):**
Mostrar o Zoom funcionando antes de explicar o código. O efeito visual impressiona e engaja a turma.

**Desconstruir o código (25 min):**
Usar o arquivo `01_campo_zoom.html` e narrar cada parte:

1. **O input com atributo `data-zoom`:** É o que ativa o componente Zoom do Style Guide
2. **O dataset vinculado:** O Zoom precisa de um dataset como fonte de dados
3. **Os campos de retorno:** Quando o usuário seleciona um registro, quais campos do formulário são preenchidos
4. **Filtro de pesquisa:** O que o usuário digita é usado como filtro no dataset

**Fala-chave:**
> "O Zoom é o componente mais usado em formulários corporativos. Seleção de fornecedor, produto, funcionário, centro de custo — tudo usa Zoom. Dominem isso e vocês resolvem 70% das necessidades."

**Erro proposital:** Configurar o Zoom com nome de dataset errado → modal abre vazio → "O que aconteceu?" → verificar nome do dataset (case sensitive).

---

### ⏱️ [00:45 - 01:15] AUTOCOMPLETE + PAI × FILHO (30 min)

#### AutoComplete (10 min)

> "AutoComplete é o irmão mais simples do Zoom. Em vez de abrir uma modal, ele mostra sugestões conforme o usuário digita — como a busca do Google."

Mostrar `02_autocomplete.html`. Explicar que o AutoComplete também consulta um dataset, mas sem a modal.

#### Técnica Pai × Filho (20 min)

> "Agora vem o conceito mais importante desta sessão. Imaginem: o usuário seleciona um Estado, e o campo Cidade carrega automaticamente só as cidades daquele estado. Isso é Pai × Filho."

**Desenhar o diagrama de sequência no quadro:**
```
Usuário seleciona "São Paulo" no campo Estado
    → JavaScript captura o evento onChange
    → Chama o dataset ds_cidades com filtro estado="SP"
    → Dataset retorna: Santos, Campinas, Sorocaba...
    → JavaScript popula o <select> de Cidade com os resultados
    → Campo Cidade limpa a seleção anterior (IMPORTANTE!)
```

**Live coding narrado** usando `03_pai_filho.html`:
1. Montar o select de Estado (dados fixos)
2. Montar o select de Cidade (vazio inicialmente)
3. Escrever o evento `onChange` do Estado
4. Dentro do onChange, consultar dataset filtrado
5. Popular o select de Cidade com o resultado

**Erro proposital:** NÃO limpar o campo filho quando o pai muda:
> "Seleciono São Paulo, aparece Santos. Agora mudo para Rio de Janeiro... e Santos ainda está selecionado como cidade! Bug clássico. Solução: sempre limpar o campo filho antes de popular com novos dados."

**Fala-chave:**
> "Pai × Filho é o padrão cascata. Categoria → Subcategoria, País → Estado → Cidade, Departamento → Setor. Sempre que um campo depende de outro, é Pai × Filho."

---

### ⏱️ [01:15 - 01:25] ☕ INTERVALO (10 min)

---

### ⏱️ [01:25 - 01:50] SELECT COM DATASET (25 min)

> "Até agora os options dos selects estavam fixos no HTML. Mas e se os departamentos mudam? E se os produtos são 500? Não dá para fixar no HTML. A solução é popular o select via dataset."

**Live coding** usando `04_select_com_dataset.html`:

1. Criar um `<select>` vazio (só com a option "Selecione...")
2. No `$(document).ready`, chamar `DatasetFactory.getDataset`
3. Iterar os resultados e criar `<option>` com jQuery
4. Adicionar ao select

**Comparativo arquitetural (10 min):**
> "Quando usar select fixo vs select com dataset?"

| Cenário | Abordagem |
|---------|-----------|
| Poucas opções que nunca mudam (Sim/Não, M/F) | Select fixo no HTML |
| Opções que podem mudar (departamentos, cargos) | Select com dataset |
| Muitas opções (500+ produtos) | Zoom, não select |

---

### ⏱️ [01:50 - 02:25] LISTAS DINÂMICAS (35 min)

> "Essa é a funcionalidade que vocês mais vão usar no dia a dia. Lista dinâmica é aquela tabela onde o usuário clica 'Adicionar' e aparece uma nova linha. Tipo os itens de uma nota fiscal."

**Live coding** usando `05_lista_dinamica.html`:

#### Conceito da convenção `___` (10 min)

> "Lembram dos 3 underscores que falei na sessão passada? É aqui que eles brilham. Campos com `___` no name pertencem a uma tabela filha no banco."

```
name="item_descricao___1"   → linha 1, coluna descricao
name="item_descricao___2"   → linha 2, coluna descricao
name="item_quantidade___1"  → linha 1, coluna quantidade
name="item_quantidade___2"  → linha 2, coluna quantidade
```

#### Construindo a lista (25 min)

1. Criar a tabela HTML com cabeçalho
2. Primeira linha com campos `___1`
3. Botão "Adicionar linha" com jQuery que clona a última linha e incrementa o sufixo
4. Botão "Remover" em cada linha
5. Cálculo automático de subtotal (quantidade × valor)
6. Cálculo do total geral

**Pontos críticos para explicar:**
- Os nomes dos campos PRECISAM seguir o padrão `nome___N`
- Ao adicionar linha, incrementar o N
- Ao remover linha, reindexar os campos (ou usar abordagem sem reindexação)
- O Fluig reconhece automaticamente a tabela filha pelo padrão `___`

**Fala-chave:**
> "A mágica está nos 3 underscores. O Fluig olha para `item_descricao___1` e `item_descricao___2` e entende: 'ah, são linhas de uma tabela filha chamada item'. Ele cria a tabela filha automaticamente no banco."

---

### ⏱️ [02:25 - 02:35] ☕ INTERVALO (10 min)

---

### ⏱️ [02:35 - 03:20] EXERCÍCIO: FORMULÁRIO COM ZOOM + LISTA (45 min)

**Instruções:**
> "Criem um formulário de Pedido de Material com:"

1. Dados do solicitante (nome via helper, departamento via select com dataset)
2. Campo Zoom para selecionar fornecedor (pode usar dataset mock)
3. Lista dinâmica de itens (descrição, quantidade, valor unitário, subtotal calculado)
4. Total geral calculado automaticamente
5. Botão adicionar/remover linhas

**Código base:** Fornecer `06_exercicio_pedido_material.html` como template inicial (com a estrutura pronta, faltando a lógica jQuery).

**Enquanto a turma trabalha:** Circular, ajudar com os 3 underscores (vai dar problema), e identificar quem avança rápido.

---

### ⏱️ [03:20 - 03:45] REVISÃO + DISCUSSÃO (25 min)

**Revisão dos exercícios (10 min):**
Pedir 2-3 alunos para mostrarem.

**Discussão arquitetural — performance (15 min):**

> "Quando vocês populam um select chamando um dataset no `$(document).ready`, o que acontece se o dataset tem 10.000 registros?"

Guiar para:
- Select com 10.000 options = browser trava
- Solução 1: Usar Zoom em vez de select para grandes volumes
- Solução 2: Usar constraints no dataset (filtrar no servidor, não no browser)
- Solução 3: Paginação (para listas dinâmicas com muitos itens)

> "Regra de ouro: select para até 50 opções, Zoom para mais que 50. E sempre use constraints no dataset."

---

### ⏱️ [03:45 - 04:00] ENCERRAMENTO (15 min)

**3 Takeaways do dia:**
1. "Zoom para busca avançada com modal, AutoComplete para sugestões inline, Select com Dataset para combos dinâmicos"
2. "Pai × Filho: sempre limpar o campo filho quando o pai muda"
3. "Lista dinâmica usa a convenção `___` (3 underscores) — o Fluig cria a tabela filha automaticamente"

**Preview do Projeto Integrador:**
> "Na segunda começa o Projeto Integrador. Vamos juntar tudo que aprendemos: formulário com Zoom de fornecedor, lista dinâmica de itens, cálculos automáticos, e a estrutura do processo de aprovação. Venham com as ideias frescas."

---

## NOTAS DO INSTRUTOR

### Se sobrar tempo
- Mostrar Zoom com múltiplos campos de retorno (preenche 3-4 campos de uma vez)
- Implementar filtro na lista dinâmica
- Discutir usabilidade: tab order, feedback visual, disabled vs readonly

### Se faltar tempo
- AutoComplete pode ser mencionado sem live coding (é parecido com Zoom)
- Exercício pode usar template mais completo (menos trabalho para o aluno)
- Pular discussão de performance e fazer na sessão 05

### Riscos da sessão
- **Dataset não existe no ambiente** → Ter código mock que retorna dados fixos
- **Zoom não abre modal** → Verificar se o componente do Style Guide está carregado
- **3 underscores confundem** → Reforçar com diagrama no quadro
- **jQuery não funciona** → Verificar se o formulário está dentro do Fluig (jQuery vem da plataforma)