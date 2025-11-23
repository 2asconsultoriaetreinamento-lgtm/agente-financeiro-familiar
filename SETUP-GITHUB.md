# Setup do Repositório no GitHub

Este guia ajuda a configurar este repositório após o clone inicial.

## 1. Branch Protection (Recomendado)
Para proteger a branch `main`:
1.  Vá em **Settings** > **Branches**.
2.  Clique em **Add branch protection rule**.
3.  Em **Branch name pattern**, digite `main`.
4.  Marque:
    -   [x] Require a pull request before merging.
    -   [x] Require status checks to pass before merging.

## 2. Secrets (Para CI/CD)
Se utilizar GitHub Actions, configure os segredos:
1.  Vá em **Settings** > **Secrets and variables** > **Actions**.
2.  Adicione:
    -   `SUPABASE_URL`
    -   `SUPABASE_ANON_KEY`

## 3. Issues e Project Board
- Utilize a aba **Projects** para criar um Kanban (To Do, In Progress, Done).
- Converta as tarefas do Roadmap (`docs/09-roadmap-futuro.md`) em Issues.
