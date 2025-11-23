# Plano de Testes

## 1. Estratégia
Foco em testes manuais críticos (E2E) e testes automatizados de fluxo (n8n).

## 2. Casos de Teste (E2E)

### 2.1. Autenticação
- [ ] Cadastro de novo usuário.
- [ ] Login com email/senha.
- [ ] Recuperação de senha.

### 2.2. CRUD de Contas
- [ ] Criar uma despesa única.
- [ ] Criar uma receita recorrente.
- [ ] Editar valor de uma conta pendente.
- [ ] Marcar conta como paga.
- [ ] Excluir conta.

### 2.3. Chatbot (Fluxo Feliz)
- [ ] Enviar: "Gastei 100 em farmácia hoje".
- [ ] Verificar se o bot confirma corretamente.
- [ ] Verificar se o registro apareceu no banco de dados.

### 2.4. Chatbot (Fluxo de Erro)
- [ ] Enviar mensagem sem sentido ("batata").
- [ ] Verificar se o bot pede clarificação.

## 3. Testes de Segurança
- [ ] Tentar acessar dados de outra família via API (esperado: erro 403/401).
- [ ] Tentar SQL Injection no chat (esperado: bot ignora ou sanitiza).
