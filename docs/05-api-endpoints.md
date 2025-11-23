# API Endpoints e Webhooks

## 1. Supabase REST API
O frontend se comunica diretamente com o Supabase via Client Library (que usa REST por baixo dos panos).

### 1.1. Accounts (Contas)
- **GET /rest/v1/accounts**
  - Headers: `apikey`, `Authorization: Bearer <user_token>`
  - Query Params: `select=*`, `family_id=eq.<id>`
  - Descrição: Lista contas da família.

- **POST /rest/v1/accounts**
  - Body: `{ description, amount, type, due_date, category, ... }`
  - Descrição: Cria nova conta.

- **PATCH /rest/v1/accounts?id=eq.<id>**
  - Body: `{ status: 'paid' }`
  - Descrição: Atualiza status ou dados.

- **DELETE /rest/v1/accounts?id=eq.<id>**
  - Descrição: Remove uma conta.

## 2. Webhooks (n8n)
O n8n expõe webhooks para receber interações do chat e processar IA.

### 2.1. Processar Mensagem do Chat
- **POST https://<n8n-instance>/webhook/chat-message**
- **Payload:**
  ```json
  {
    "userId": "uuid",
    "familyId": "uuid",
    "message": "Gastei 30 reais no uber",
    "timestamp": "ISO-8601"
  }
  ```
- **Retorno:**
  ```json
  {
    "reply": "Entendido! Registrei uma despesa de R$ 30,00 na categoria Transporte.",
    "action": "create_expense",
    "data": { ... }
  }
  ```

### 2.2. Agendamento Diário (Cron)
- **Fluxo Interno no n8n** (não exposto publicamente)
- **Trigger:** Todos os dias às 08:00.
- **Ação:** Verifica contas vencendo hoje e envia notificação (via Push/Email) para os usuários da família.
