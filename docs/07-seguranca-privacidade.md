# Segurança e Privacidade

## 1. Autenticação e Autorização
- **Supabase Auth:** Gerencia usuários. Suporte a Email/Senha e OAuth (Google).
- **JWT (JSON Web Tokens):** Todas as requisições para a API devem conter um JWT válido.

## 2. Row Level Security (RLS)
A principal camada de segurança dos dados. Garante que uma família não acesse dados de outra.

### 2.1. Regra de Isolamento
```sql
-- Exemplo de Policy para tabela accounts
CREATE POLICY "Famílias veem apenas suas contas"
ON public.accounts
FOR ALL
USING (
  family_id IN (
    SELECT family_id FROM public.profiles
    WHERE id = auth.uid()
  )
);
```

## 3. Proteção de Dados (LGPD)
- **Minimização de Dados:** Coletamos apenas o necessário (Nome, Email, Foto).
- **Direito de Esquecimento:** Usuário pode solicitar exclusão da conta. Um fluxo de exclusão em cascata remove `profile` -> `family` -> `accounts`.
- **Criptografia:**
  - Em trânsito: HTTPS/TLS em todas as conexões.
  - Em repouso: Banco de dados criptografado pelo provedor (Supabase).

## 4. Auditoria
- Tabela `audit_logs` (futuro) registrará ações críticas (exclusão de registros, alteração de permissões).
