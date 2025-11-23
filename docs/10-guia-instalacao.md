# Guia de Instalação e Configuração

## 1. Pré-requisitos
- Node.js v18+
- Conta no Supabase
- Instância do n8n (Local via Docker ou Cloud)

## 2. Supabase Setup
1.  Crie um projeto no Supabase.
2.  Vá em `SQL Editor` e rode o script de `04-modelagem-dados.md` (ou use migrations).
3.  Copie `SUPABASE_URL` e `SUPABASE_ANON_KEY`.

## 3. Frontend (React)
1.  Clone o repositório:
    ```bash
    git clone https://github.com/seu-repo/agente-financeiro.git
    cd frontend
    ```
2.  Instale dependências: `npm install`
3.  Crie `.env`:
    ```env
    VITE_SUPABASE_URL=...
    VITE_SUPABASE_ANON_KEY=...
    ```
4.  Rode localmente: `npm run dev`

## 4. n8n Setup
1.  Importe o workflow do agente financeiro (arquivo JSON na pasta `workflows/`).
2.  Configure as credenciais do Supabase e OpenAI no n8n.
3.  Ative o workflow.
