# Especificação Técnica

## 1. Arquitetura do Sistema
O sistema segue uma arquitetura moderna, serverless e orientada a eventos.

### 1.1. Componentes Principais
1.  **Frontend (Client-Side):**
    -   **Tecnologia:** React.js (Vite) + TypeScript.
    -   **UI Kit:** Shadcn/ui + Tailwind CSS.
    -   **Hospedagem:** Vercel ou Google Antigravity.
    -   **Responsabilidade:** Interface do usuário, gestão de estado local, chamadas API.

2.  **Backend / Banco de Dados:**
    -   **Tecnologia:** Supabase (BaaS).
    -   **Banco:** PostgreSQL.
    -   **Auth:** Supabase Auth (Email/Senha, Google).
    -   **Segurança:** Row Level Security (RLS) para isolamento de dados.

3.  **Automação e IA:**
    -   **Tecnologia:** n8n (Self-hosted ou Cloud).
    -   **LLM:** OpenAI (GPT-4o-mini) ou Anthropic (Claude 3 Haiku) via API.
    -   **Responsabilidade:** Processamento de linguagem natural, categorização automática, agendamento de tarefas.

## 2. Fluxo de Dados

### 2.1. Registro de Despesa via Chat
1.  Usuário envia mensagem: "Gastei 50 no mercado".
2.  Frontend envia para Webhook do n8n.
3.  n8n processa com LLM -> Extrai JSON `{valor: 50, categoria: "Mercado", data: "hoje"}`.
4.  n8n insere registro no Supabase via API REST.
5.  n8n responde ao frontend: "Despesa registrada!".

### 2.2. Consulta de Dashboard
1.  Frontend solicita dados ao Supabase (Client Library).
2.  Supabase verifica RLS (Token JWT).
3.  Supabase retorna apenas dados da família do usuário.
4.  Frontend renderiza gráficos.

## 3. Modelo de Dados (ER Simplificado)

-   **users (auth.users):** Tabela padrão do Supabase.
-   **profiles:** Dados estendidos do usuário (nome, telefone, family_id).
-   **accounts:** Contas a pagar/receber.
    -   `id` (uuid)
    -   `description` (text)
    -   `amount` (numeric)
    -   `due_date` (date)
    -   `category` (text)
    -   `status` (enum: pending, paid)
    -   `type` (enum: expense, income)
    -   `user_id` (fk)
    -   `family_id` (fk)
