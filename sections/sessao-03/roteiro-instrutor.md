# SESSÃO 03 — Formulários Customizados e Dinâmicos — Fundamentos
## Roteiro do Instrutor | 24/03/2026 (Segunda) | 4 horas

---

## OBJETIVO DA SESSÃO
Construção de formulários dinâmicos e bem estruturados usando HTML + recursos nativos do Fluig. Ao final, o aluno entende a anatomia de um formulário Fluig, domina os principais tipos de input, aplica validações, e constrói um formulário funcional do zero.

---

## PREPARAÇÃO (Antes da aula)

### Checklist
- [ ] Ambiente Fluig com logins dos alunos funcionando (criados na semana passada)
- [ ] Eclipse + fluig Studio configurado e testado (sessão 02 deve ter resolvido)
- [ ] Códigos de exemplo da sessão prontos (pasta `codigos/`)
- [ ] Formulário "com erro proposital" (sem `name`) pré-preparado para demo
- [ ] Slides abertos e testados

### Material de Apoio
- Slides: `sessao-03/slides.pptx`
- Códigos: `sessao-03/codigos/`
- Cola: `sessao-03/README.md`
- Mini site alunos: `sessao-03/index.html`

---

## TIMELINE DETALHADA

---

### ⏱️ [00:00 - 00:15] RECONEXÃO (15 min)

**O que fazer:**
1. Pedir para um aluno resumir a sessão 02 (Eclipse, Studio, deploy, Style Guide)
2. Verificar rapidamente: "Todo mundo conseguiu configurar o ambiente? Alguém teve problema?"
3. Conectar com hoje: "Na sessão passada configuramos a ferramenta. Hoje vamos usar ela pra valer."

**Fala sugerida:**
> "Quem resume pra gente o que fizemos na quinta? O que é o fluig Studio, como faz deploy, e o que é o Style Guide?"

Após o resumo:
> "Perfeito. Hoje é a sessão mais importante do bloco de formulários. Tudo que vocês vão construir no Fluig começa com um formulário bem feito. Vamos entender a anatomia, todos os tipos de campo, validações, e no final cada um vai construir o seu."

---

### ⏱️ [00:15 - 00:45] ANATOMIA DE UM FORMULÁRIO FLUIG (30 min)

**O que fazer:**
Explicar a estrutura obrigatória de um formulário Fluig, comparando com HTML puro.

#### Comparativo HTML puro vs Fluig (15 min)

Mostrar lado a lado:

**Fala sugerida:**
> "Um formulário Fluig É um HTML. Não é um framework proprietário, não é uma linguagem diferente. É HTML com algumas convenções obrigatórias. Vou mostrar as diferenças."

Mostrar o código `01_html_puro_vs_fluig.html` da pasta de códigos.

Pontos para explicar:

1. **Wrapper obrigatório:** `<div class="fluig-style-guide">` — sem isso, os componentes do Style Guide não funcionam
2. **Atributo `name` é sagrado:** Cada campo que precisa salvar dados PRECISA ter `name`. Sem `name` = dado perdido
3. **Convenção de `name`:** snake_case, sem acentos, sem espaços. Ex: `nome_completo`, `data_nascimento`
4. **Sem tag `<form>` explícita:** O Fluig gerencia o submit automaticamente — você não escreve `<form action="...">`. O Fluig envolve seu HTML num form internamente
5. **Sem Bootstrap CDN:** O Style Guide já é carregado pela plataforma. Nunca importar Bootstrap via CDN — gera conflito

**⚡ Gatilho arquitetural:**
> "Por que vocês acham que a TOTVS escolheu usar HTML puro em vez de criar um framework de formulários próprio, tipo um drag-and-drop?"

Guiar para:
- **Vantagem:** Flexibilidade total. Qualquer coisa que funciona em HTML funciona no Fluig
- **Vantagem:** Desenvolvedor web já sabe HTML — curva de aprendizado menor
- **Desvantagem:** Sem validação automática de estrutura — erros silenciosos (ex: campo sem `name`)
- **Desvantagem:** Desenvolvedor precisa conhecer as convenções

#### Erro Proposital: Campo sem `name` (15 min)

**O que fazer:**
1. Abrir o formulário `02_erro_sem_name.html` no Eclipse
2. Mostrar: dois campos, um COM `name` e outro SEM
3. Deployar no Fluig
4. Preencher ambos os campos, salvar
5. Abrir o registro — primeiro campo salvou, segundo está vazio

**Fala sugerida:**
> "Olhem: eu preenchi os dois campos. Salvei. Agora vou abrir o registro. O campo 'Nome' salvou perfeitamente. O campo 'Apelido'... sumiu. Por quê?"

Deixar a turma responder. Depois explicar:

> "O Fluig usa o atributo `name` como endereço no banco de dados. Quando tem `name='nome_completo'`, ele cria uma coluna 'nome_completo' e guarda o valor lá. Sem `name`, ele não sabe onde guardar. O dado é preenchido na tela mas nunca chega no banco. É o erro número 1 de quem começa no Fluig."

---

### ⏱️ [00:45 - 01:15] LIVE CODING: FORMULÁRIO CAMPO A CAMPO (30 min)

**O que fazer:**
Construir o formulário `03_formulario_cadastro.html` do zero, narrando cada decisão.

**Ordem de construção:**

1. **(3 min)** Estrutura base: `<div class="fluig-style-guide">` + primeiro `panel`
2. **(5 min)** Campo texto: `<input type="text" name="nome_completo">` + `form-group` + `label`
3. **(5 min)** Campo email: `<input type="email" name="email">` — explicar que `type="email"` dá validação nativa do browser
4. **(5 min)** Campo data: `<input type="date" name="data_admissao">` — explicar renderização nativa do browser
5. **(5 min)** Select: `<select name="departamento">` + options — explicar que o `value` da option é o que salva, não o texto
6. **(4 min)** Radio: `<input type="radio" name="prioridade" value="alta">` — explicar que radios com mesmo `name` formam grupo
7. **(3 min)** Checkbox: `<input type="checkbox" name="urgente" value="S">` — explicar que só envia se marcado

**Dica para o instrutor:** A cada campo, faça deploy e teste. Mostrar o dado salvando campo a campo é mais didático do que construir tudo e testar no final.

**Fala para cada campo:**
> "Notem o padrão: sempre `form-group` por fora, `label` para acessibilidade, e o input com `class='form-control'` para o estilo do Style Guide. E o mais importante: o `name`, que é nosso contrato com o banco."

---

### ⏱️ [01:15 - 01:25] ☕ INTERVALO (10 min)

---

### ⏱️ [01:25 - 01:55] HELPERS DO FLUIG + VALIDAÇÕES (30 min)

#### Helpers do Fluig (15 min)

**O que são:**
Helpers são funções JavaScript disponibilizadas pelo Fluig que facilitam operações comuns dentro de formulários.

Mostrar os mais usados:

```javascript
// Pegar dados do usuário logado
WCMAPI.getUser()          // retorna login do usuário
WCMAPI.getUserCode()      // retorna código do usuário

// Verificar modo do formulário
document.getElementById("___FORM_MODE___")  // "ADD", "MOD", "VIEW"

// Pegar número do documento
document.getElementById("___NUM_DOCUMENT___")
```

**Fala sugerida:**
> "Esses helpers são atalhos que o Fluig disponibiliza. O mais usado é o `WCMAPI.getUser()` — vocês vão usar ele em praticamente todo formulário para preencher automaticamente o nome do solicitante."

Demonstrar ao vivo: preencher campo de solicitante automaticamente com `WCMAPI.getUser()`.

#### Validações básicas (15 min)

Mostrar três níveis de validação:

**Nível 1 — HTML nativo:**
```html
<input type="text" name="nome" required>
<input type="email" name="email">  <!-- valida formato -->
<input type="number" name="quantidade" min="1" max="100">
```

**Nível 2 — jQuery no browser:**
```javascript
$('#btn-salvar').click(function() {
    if ($('#nome').val().trim() === '') {
        alert('Nome é obrigatório!');
        return false;
    }
});
```

**Nível 3 — Máscara com jQuery Mask (se tiver tempo):**
```javascript
$('#cpf').mask('000.000.000-00');
$('#telefone').mask('(00) 00000-0000');
$('#cep').mask('00000-000');
```

**Fala-chave:**
> "Validação HTML é a primeira linha de defesa — é grátis e funciona sem JavaScript. jQuery é a segunda linha — para regras de negócio mais complexas. E nas próximas sessões vamos ver o `validateForm`, que é a validação no servidor — a linha de defesa final."

---

### ⏱️ [01:55 - 02:35] EXERCÍCIO GUIADO: FORMULÁRIO DE CADASTRO (40 min)

**O que fazer:**
Toda a turma constrói junto o formulário `04_exercicio_guiado_cadastro.html`. Instrutor narra, turma acompanha.

**Formulário: Cadastro de Funcionário**

Campos a construir:
1. Nome Completo (text, obrigatório)
2. E-mail Corporativo (email)
3. CPF (text, com máscara)
4. Data de Nascimento (date)
5. Departamento (select com 5 opções)
6. Cargo (text)
7. Tipo de Contrato (radio: CLT / PJ / Estágio)
8. Aceita termos (checkbox)
9. Observações (textarea)
10. Botões: Salvar + Cancelar

**Estrutura visual:**
- Panel "Dados Pessoais" (campos 1-4)
- Panel "Dados Profissionais" (campos 5-7)
- Panel "Observações" (campos 8-10)
- Grid: 2 colunas para campos curtos, 1 coluna para textarea

**Dinâmica:** Instrutor projeta a tela, vai construindo campo a campo, turma replica no próprio Eclipse. A cada 3-4 campos, faz deploy e testa.

**Se a turma estiver rápida:** Adicionar mais campos ou estilizações.
**Se a turma estiver lenta:** Focar nos 6 primeiros campos e pular máscara.

---

### ⏱️ [02:35 - 02:45] ☕ INTERVALO (10 min)

---

### ⏱️ [02:45 - 03:20] EXERCÍCIO AUTÔNOMO (35 min)

**Instruções para a turma:**

> "Agora é com vocês. Criem um formulário sobre qualquer tema — pode ser pedido de material, abertura de chamado, cadastro de cliente, o que quiserem. Requisitos mínimos:"

1. Pelo menos 8 campos de 3 tipos diferentes
2. Pelo menos 2 painéis (panels)
3. Grid com pelo menos uma linha de 2 colunas
4. Pelo menos 1 validação (required ou jQuery)
5. Botões Salvar e Cancelar
6. **Todos os campos com `name` em snake_case**

**Enquanto a turma trabalha:**
- Circular pela sala
- Ajudar quem travar no deploy
- Observar padrões de erro (anotar para revisão coletiva)
- Identificar formulários criativos para mostrar depois

**Desafio extra (devs avançados):**
- Adicionar máscara de CPF/CNPJ com jQuery Mask
- Implementar validação dinâmica com jQuery (ex: campo X obrigatório apenas se campo Y = "Sim")
- Formulário responsivo que funciona em mobile

---

### ⏱️ [03:20 - 03:45] REVISÃO + DISCUSSÃO ARQUITETURAL (25 min)

**O que fazer:**
1. **(10 min)** Pedir para 2-3 alunos mostrarem seus formulários
2. **(10 min)** Discutir escolhas: "Por que usou select aqui em vez de radio? Faz sentido?"
3. **(5 min)** Mostrar erros comuns que observou durante o exercício

**Perguntas para discussão:**
- "Quando usar select vs radio?" → Select para muitas opções (5+), radio para poucas (2-4)
- "Quando usar text vs textarea?" → Text para dados curtos (nome, código), textarea para texto livre
- "O que acontece se dois campos tiverem o mesmo `name`?" → O segundo sobrescreve o primeiro

**Fala-chave:**
> "Formulário no Fluig não é sobre saber HTML — é sobre saber as convenções. O `name` correto, a estrutura `form-group`, o Style Guide. Quem domina essas convenções constrói qualquer formulário em minutos."

---

### ⏱️ [03:45 - 04:00] ENCERRAMENTO (15 min)

**3 Takeaways do dia:**
1. "Todo campo que precisa salvar dados PRECISA ter `name` em snake_case"
2. "Estrutura padrão: `fluig-style-guide` → `panel` → `row` → `col-md-X` → `form-group` → `label` + `input`"
3. "Validação em camadas: HTML nativo (required) → jQuery (regras de negócio) → servidor (validateForm, na próxima sessão)"

**Preview da próxima sessão:**
> "Na quinta vamos subir o nível: Zoom, AutoComplete, técnica Pai × Filho, e listas dinâmicas — aquelas tabelas onde o usuário adiciona e remove linhas. É onde formulários ficam realmente poderosos."

---

## NOTAS DO INSTRUTOR

### Se sobrar tempo (15+ min)
- Aprofundar em máscaras jQuery Mask (CPF, CNPJ, telefone, CEP)
- Mostrar o evento `displayFields` como preview da sessão 07
- Discutir acessibilidade em formulários Fluig (labels, tabindex, aria)

### Se faltar tempo
- Comprimir o exercício autônomo para 20 min
- Pular a revisão coletiva e fazer por escrito (feedback individual)
- Helpers do Fluig pode ser resumido em 5 min (só mostrar WCMAPI.getUser)

### Riscos da sessão
- **Aluno sem ambiente configurado** → Deve ter sido resolvido na sessão 02. Se persistir, parear com colega
- **Deploy lento para a turma toda** → Fazer deploys escalonados (metade de cada vez)
- **jQuery Mask não carrega** → Não é nativo do Fluig, precisa incluir o JS no resources. Ter o arquivo pronto