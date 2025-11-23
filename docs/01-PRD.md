# Product Requirements Document (PRD) - Sistema Financeiro Familiar Inteligente

## 1. Visão Geral
Sistema de gestão financeira familiar que combina controle de contas (CRUD) com um agente de IA consultivo e operacional. O objetivo é centralizar a vida financeira da família, oferecendo organização, insights e educação financeira.

## 2. Objetivos
- Centralizar contas a pagar e receber.
- Oferecer um assistente virtual (chatbot) que orienta e executa tarefas.
- Promover educação financeira e redução de dívidas.

## 3. Público-Alvo
- Famílias e casais que buscam organização financeira.
- Pessoas com dificuldade em manter planilhas manuais.

## 4. Funcionalidades Principais
### 4.1. Agente Financeiro (IA)
- Interface conversacional (Chat).
- Tira dúvidas sobre finanças.
- Executa comandos: "Agendar conta de luz R$ 100 para dia 10".
- Analisa gastos e sugere economias.

### 4.2. Gestão de Contas (CRUD)
- Cadastro de Contas a Pagar e Receber.
- Baixa de lançamentos (marcar como pago).
- Categorização (Alimentação, Moradia, Lazer, etc.).
- Recorrência (mensal, semanal).

### 4.3. Dashboards e Relatórios
- Visão geral de saldo e fluxo de caixa.
- Gráficos de despesas por categoria.
- Comparativo Receita x Despesa.

## 5. Arquitetura Técnica
- **Frontend:** Interface conversacional e dashboards (React/Google Antigravity).
- **Backend/Automação:** n8n para orquestração de fluxos e IA.
- **Banco de Dados:** Supabase (PostgreSQL) com RLS.

## 6. Roadmap
- **V1.0:** Chatbot + CRUD básico + Autenticação.
- **V1.1:** Dashboards avançados.
- **V2.0:** Integração bancária (Open Finance).
