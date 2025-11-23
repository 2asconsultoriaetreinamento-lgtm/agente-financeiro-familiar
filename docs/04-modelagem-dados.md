# Modelagem de Dados

## 1. Diagrama Entidade-Relacionamento (ER)
O banco de dados utiliza PostgreSQL (Supabase). A estrutura principal foca em isolamento de dados por família (RLS).

## 2. Tabelas

### 2.1. public.profiles
Estende a tabela `auth.users` do Supabase.
- `id` (uuid, PK): Referencia `auth.users.id`.
- `full_name` (text): Nome completo.
- `avatar_url` (text): URL da foto de perfil.
- `family_id` (uuid): ID da família a qual o usuário pertence.
- `created_at` (timestamptz): Data de criação.

### 2.2. public.families
Agrupa usuários.
- `id` (uuid, PK): Identificador único.
- `name` (text): Nome da família (ex: "Família Silva").
- `created_at` (timestamptz).

### 2.3. public.accounts (Lançamentos)
Armazena todas as contas a pagar e receber.
- `id` (uuid, PK).
- `family_id` (uuid, FK): Referencia `families.id`.
- `user_id` (uuid, FK): Quem criou o lançamento.
- `description` (text): Descrição da conta (ex: "Conta de Luz").
- `amount` (numeric): Valor monetário (positivo).
- `type` (text): 'expense' (despesa) ou 'income' (receita).
- `status` (text): 'pending' (pendente) ou 'paid' (pago).
- `due_date` (date): Data de vencimento.
- `payment_date` (timestamptz, nullable): Data do pagamento real.
- `category` (text): Categoria (ex: "Moradia", "Lazer").
- `is_recurring` (boolean): Se é recorrente.
- `created_at` (timestamptz).

## 3. Políticas de Segurança (RLS)

### 3.1. Leitura (Select)
- Usuário pode ler `accounts` ONDE `family_id` = `auth.uid() -> profile.family_id`.

### 3.2. Escrita (Insert/Update/Delete)
- Usuário pode criar/editar `accounts` SE `family_id` corresponder à sua família.
