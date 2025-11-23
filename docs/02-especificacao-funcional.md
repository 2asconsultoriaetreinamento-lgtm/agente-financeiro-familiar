# 02 - Especificação Funcional Detalhada
## Sistema Financeiro Familiar Inteligente

---
02-especificacao-funcional.md
## 📌 Introdução
# Especificação Funcional

## 1. Introdução
Este documento detalha as funcionalidades do Sistema Financeiro Familiar, descrevendo fluxos de usuário, regras de negócio e comportamento esperado do sistema e do agente.

## 2. Gestão de Contas (CRUD)

### 2.1. Cadastro de Contas
- **Entradas:** Descrição, Valor, Data de Vencimento, Categoria, Recorrência (Única, Mensal, Semanal), Tipo (Pagar/Receber), Status (Pendente/Pago).
- **Regras:**
    - Valor deve ser positivo.
    - Data de vencimento obrigatória.
    - Se recorrente, gerar lançamentos futuros automaticamente (ou sob demanda).

### 2.2. Listagem e Filtros
- Visualização em tabela ou lista.
- Filtros por: Mês/Ano, Categoria, Status, Tipo.
- Ordenação por data ou valor.

### 2.3. Edição e Exclusão
- Permite editar qualquer campo de um lançamento pendente.
- Para lançamentos recorrentes: opção de editar "apenas este" ou "este e futuros".
- Exclusão lógica (soft delete) para histórico.

## 3. Agente Financeiro (Chatbot)

### 3.1. Consultas em Linguagem Natural
- **Usuário:** "Quanto gastei com mercado este mês?"
- **Agente:** Consulta banco de dados, soma valores da categoria 'Mercado' no mês atual e responde.

### 3.2. Comandos de Ação
- **Usuário:** "Adicionar despesa de R$ 50 na padaria hoje."
- **Agente:** Identifica intenção 'ADD_EXPENSE', extrai entidades (Valor: 50, Categoria: Padaria/Alimentação, Data: Hoje) e confirma a criação.

### 3.3. Insights Proativos
- Alertas de contas a vencer no dia.
- Aviso se o gasto em uma categoria exceder a média histórica.

## 4. Dashboards

### 4.1. Visão Geral
- Card de Saldo Atual.
- Card de Receitas do Mês vs Despesas do Mês.
- Gráfico de barra de gastos diários.

### 4.2. Análise por Categoria
- Gráfico de pizza (Donut) distribuindo despesas por categoria.

## 5. Regras de Negócio Globais
- Todo usuário pertence a uma "Família" (mesmo que seja unitária).
- Dados são isolados por Família (RLS).
- Moeda padrão: BRL (R$).
Este documento detalha todas as funcionalidades, regras de negócio, campos, validações e comportamentos esperados do sistema financeiro familiar inteligente.

---

## 1️⃣ Autenticação e Gerenciamento de Usuário

### 1.1 Registro de Usuário

**Fluxo:**
1. Usuário acessa página de registro
2. Preenche: nome, email, senha, confirmação de senha
3. Aceita termos de uso e LGPD
4. Sistema valida dados e cria conta

**Validações:**
- Email válido e único
- Senha com mínimo 8 caracteres (números, maiúsculas, minúsculas)
- Campos obrigatórios preenchidos

**Resposta de sucesso:**
- Conta criada
- Email de confirmação enviado (opcional para MVP)
- Redirecionado para dashboard

---

### 1.2 Login

**Fluxo:**
1. Usuário insere email e senha
2. Sistema valida credenciais via Supabase Auth
3. JWT token gerado
4. Sessão iniciada

**Regras:**
- Máximo 5 tentativas de login falhadas antes de bloqueio temporário
- Sessão expira após 24 horas de inatividade
- "Lembrar-me" mantém sessão por 30 dias

---

### 1.3 Perfis de Acesso

| Perfil | Permissões |
|--------|-----------|
| **Proprietário** | Criar/editar/deletar lançamentos, contas, usuários; Acessar relatórios; Gerenciar permissões |
| **Membro** | Criar/editar lançamentos; Visualizar dados; Não pode deletar nem gerenciar usuários |
| **Visualizador** | Apenas ler dados; Sem permissão de criar/editar |

---

## 2️⃣ Agente Financeiro - Chat Conversacional

### 2.1 Características do Agente

**System Message (Prompt):**
```
Você é um consultor financeiro familiar motivador, especialista em 
orçamento doméstico, controle de gastos, investimentos básicos, educação 
financeira para crianças e várias áreas da vida financeira. Seu foco 
principal é orientar famílias e pessoas a saírem das dívidas e promover o 
conhecimento financeiro de forma acessível, prática e inspiradora, sempre 
incentivando uma relação saudável com o dinheiro em todas as idades.
```

### 2.2 Tipos de Respostas

#### Tipo A: Orientação Educativa
- Dicas sobre gestão de dinheiro
- Sugestões para reduzir despesas
- Estratégias de saída de dívidas
- Educação financeira para crianças

**Exemplo de prompt do usuário:**
> "Como posso sair de dívidas de forma inteligente?"

**Resposta esperada:**
> "Excelente pergunta! Aqui está um plano motivador para você:
> 1. **Faça um diagnóstico**: Liste todas as dívidas com juros
> 2. **Crie prioridades**: Comece pelas com maior taxa de juros
> 3. **Organize o orçamento**: Dedique % da receita para pagar
> 4. **Pequenas vitórias**: Comemore cada dívida zerada!
> 
> Quer que eu registre suas dívidas aqui para acompanhamos juntos? 💪"

#### Tipo B: Execução de Comando
- Cadastrar lançamento
- Consultar dados
- Gerar relatório rápido

**Exemplo:**
> "Cadastre uma despesa de R$ 150 com água hoje"

**Resposta esperada:**
> "Perfeito! Registrei sua despesa:
> - **Descrição**: Água
> - **Valor**: R$ 150
> - **Data**: Hoje
> - **Status**: Registrado ✓
> 
> Seus gastos com contas agora somam R$ XXX este mês."

#### Tipo C: Consulta de Dados
- "Quanto gastei em alimentação?"
- "Qual é meu saldo atual?"
- "Quantas contas faltam pagar?"

**Resposta esperada:**
> "Seus gastos com alimentação em novembro:
> - **Total**: R$ 1.250
> - **Transações**: 12
> - **Média por dia**: R$ 40,30
> 
> Sugestão: Está um pouco alto. Quer dicas para economizar? 💡"

### 2.3 Feedback do Chat

- Usuário pode avaliar cada resposta (👍 Útil / 👎 Não útil)
- Respostas negativas geram alert para revisão

---

## 3️⃣ CRUD de Lançamentos Financeiros

### 3.1 Estrutura de Dados

#### Lançamento (Movimentação)
```
{
  id: UUID (primary key)
  usuario_id: UUID (foreign key → usuario)
  familia_id: UUID (foreign key → familia)
  tipo: ENUM ['entrada', 'saída']
  descricao: STRING (obrigatório, max 255 caracteres)
  valor: DECIMAL (obrigatório, > 0)
  categoria_id: UUID (foreign key → categoria)
  data: DATE (obrigatório)
  status: ENUM ['pago', 'pendente', 'cancelado']
  notas: TEXT (opcional)
  tags: ARRAY[STRING] (opcional, para busca rápida)
  criado_em: TIMESTAMP
  atualizado_em: TIMESTAMP
}
```

#### Categoria
```
{
  id: UUID
  nome: STRING (obrigatório, unique)
  icone: STRING (emoji ou ícone)
  cor: HEX_COLOR
  tipo: ENUM ['despesa', 'receita']
  usuario_id: UUID (NULL se pré-definida)
}
```

### 3.2 Operações CRUD

#### CREATE - Cadastrar Lançamento

**Campos obrigatórios:**
- Descrição
- Valor
- Categoria
- Data
- Tipo (entrada/saída)

**Campos opcionais:**
- Notas
- Tags
- Status (padrão: pendente)

**Validações:**
- Valor > 0
- Data não pode ser no futuro (padrão: hoje)
- Categoria deve existir
- Descrição não pode estar vazia

**Fluxo:**
1. Usuário clica "Novo Lançamento"
2. Preenche formulário
3. Sistema valida
4. Salva em Supabase
5. Dashboard atualiza em tempo real
6. Agente sumariza: "Registrei R$ XXX em [categoria]. Total gasto no mês: R$ YYY"

#### READ - Visualizar Lançamentos

**Listagem com filtros:**
- Por período (data inicial/final)
- Por categoria
- Por tipo (entrada/saída)
- Por status (pago/pendente)
- Busca por descrição

**Ordenação:**
- Data (mais recente primeiro)
- Valor (maior/menor)
- Categoria

**Paginação:**
- 20 itens por página

#### UPDATE - Editar Lançamento

**Campos editáveis:**
- Descrição
- Valor
- Categoria
- Data
- Status
- Notas
- Tags

**Validações:** Mesmas do CREATE

**Auditoria:**
- Registra quem alterou e quando
- Permite reverter para versão anterior

#### DELETE - Deletar Lançamento

**Regras:**
- Apenas proprietário da família pode deletar
- Soft delete (marca como deletado, não remove do banco)
- Permite recuperação por 30 dias

---

## 4️⃣ Categorias de Gastos

### 4.1 Categorias Pré-definidas

```
DESPESAS:
- Alimentação
- Transporte
- Saúde
- Educação
- Moradia
- Utilidades (água, luz, gás)
- Diversão
- Compras
- Impostos
- Outras

RECEITAS:
- Salário
- Freelance
- Investimentos
- Bônus
- Outras
```

### 4.2 Categorias Customizadas

- Usuário pode criar categorias personalizadas
- Pode editar/deletar apenas suas categorias
- Máximo 50 categorias por usuário

---

## 5️⃣ Dashboard Principal

### 5.1 Componentes

#### Saldo Atual
- Total de entradas no mês
- Total de saídas no mês
- Saldo líquido
- Comparativo com mês anterior (↑/↓ %)

#### Resumo de Contas
- Contas a pagar: # de contas + valor total
- Contas a receber: # de contas + valor total
- Status visual (verde se tudo ok, vermelho se vencidas)

#### Gráfico: Receita vs Despesa
- Período selecionável (mês/trimestre/ano)
- Gráfico de barras comparativo
- Legenda com valores

#### Últimos Lançamentos
- Tabela com 5 últimos registros
- Colunas: Data, Descrição, Categoria, Valor, Status
- Link para "Ver todos"

#### Card de Alerta (se houver)
- "Você tem 3 contas vencidas"
- "Seus gastos ultrapassaram a meta"

---

## 6️⃣ Funcionalidades Futura (V1.1+)

### Dashboards Avançados
- Análise por categoria (pizza chart)
- Comparativo temporal
- Previsão de fluxo de caixa
- Metas financeiras

### Notificações
- Lembretes de contas vencidas
- Alertas de categorias excedidas
- Sugestões do agente

### Relatórios
- PDF mensal/anual
- Excel com dados brutos
- Email automático

---

## 7️⃣ Regras de Negócio Gerais

1. **Cada usuário só vê seus dados** (RLS)
2. **Dados são armazenados em UTC** (convertidos para localtime no frontend)
3. **Operações são atômicas** (tudo ou nada)
4. **Auditoria completa** (quem fez, quando, o quê)
5. **Validação client-side + server-side**
6. **Cache de 1 minuto para dashboards** (performance)

---

**Última atualização**: Novembro 2025
