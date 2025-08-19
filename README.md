# Emprega Viçosa - Backend

API desenvolvida em Laravel para gerenciamento de usuários, perfis, experiências, serviços e publicações.

---

## Sumário

- [Setup Local (Manual)](#setup-local-manual)
- [Setup com Docker](#setup-com-docker)
- [Comandos Úteis](#comandos-úteis)
- [Testes Unitários](#testes-unitários)
- [Erros Comuns](#erros-comuns)
- [Migração do Docker para Local](#migração-do-docker-para-local)
- [Documentação da API](#documentação-da-api)

---

## Setup Local (Manual)

1. **Crie o banco de dados com Docker:**
   ```sh
   docker run --name emprega -e POSTGRESQL_USERNAME=admin -e POSTGRESQL_PASSWORD=admin -e POSTGRESQL_DATABASE=emprega_api -p 5432:5432 bitnami/postgresql
   ```

2. **(Opcional) Visualize os dados com DBeaver:**
   - Crie uma nova conexão PostgreSQL usando as credenciais acima.

3. **Clone o projeto:**
   ```sh
   git clone https://github.com/MarcelCNoronha/empregavicosa-back
   cd empregavicosa-back
   code . # Abre o VSCode na raiz do projeto
   ```

4. **Instale as dependências:**
   ```sh
   composer install
   ```

5. **Configure o ambiente:**
   - Copie `.env.example` para `.env`.

6. **Execute as migrações e seeds:**
   ```sh
   php artisan migrate
   php artisan db:seed
   ```

7. **Gere a APP KEY:**
   ```sh
   php artisan key:generate
   ```

8. **Inicie o servidor:**
   ```sh
   php artisan serve
   ```

---

## Setup com Docker

1. **Clone o projeto:**
   ```sh
   git clone https://github.com/MarcelCNoronha/empregavicosa-back
   cd empregavicosa-back
   code .
   ```

2. **Suba os containers e crie o banco:**
   ```sh
   docker compose up -d --build
   ```

3. **Execute o setup completo:**
   ```sh
   docker compose exec php composer setup
   ```

4. **Inicie o servidor:**
   ```sh
   composer serve
   ```

---

## Comandos Úteis

- **Parar containers Docker:**
  ```sh
  docker compose stop
  ```

- **Reiniciar containers Docker:**
  ```sh
  docker compose up
  ```

- **Acessar o bash do container:**
  ```sh
  docker compose exec php bash
  ```

- **Executar migrações:**
  ```sh
  composer migrate
  ```

- **Reverter migrações:**
  ```sh
  composer rollback
  ```

- **Desativar completamente a virtualização:**
  ```sh
  docker compose down
  ```
  > **Atenção:** Este comando apaga todos os dados e configurações dos containers.

---

## Testes Unitários

1. **Configure o SQLite em memória no `phpunit.xml`:**
   ```xml
   <env name="DB_CONNECTION" value="sqlite" />
   <env name="DB_DATABASE" value=":memory:" />
   ```

2. **Atualize o banco de testes:**
   ```sh
   php artisan migrate:fresh --seed --env=testing
   ```

3. **Execute os testes:**
   ```sh
   docker compose exec php php artisan test --coverage --env=testing
   ```

---

## Erros Comuns

- **Erro de seed/teste por email null:**
  - Acesse o bash do container:
    ```sh
    docker compose exec php bash
    ```
  - Execute:
    ```sh
    php artisan config:clear
    php artisan route:clear
    php artisan route:cache
    ```

---

## Migração do Docker para Local

Se quiser voltar a rodar localmente, altere no `.env`:
```env
DB_HOST=127.0.0.1
```
Depois, siga o [Setup Local (Manual)](#setup-local-manual).

---

## Documentação da API

### Autenticação

- `POST /api/authenticate` — Autentica um usuário.
- `GET /api/user/authenticate` — Retorna o usuário autenticado.
- `GET /api/user/is-user/{email}` — Verifica se o e-mail pertence a um usuário.

### Usuários

- `POST /api/users` — Cria um novo usuário.

### Perfis

- `GET /api/perfils` — Lista todos os perfis.
- `POST /api/perfils` — Cria um novo perfil.
- `GET /api/perfils/{id}` — Exibe um perfil.
- `PUT /api/perfils/{id}` — Atualiza um perfil.
- `DELETE /api/perfils/{id}` — Remove um perfil.

### Serviços

- `GET /api/services` — Lista todos os serviços.
- `POST /api/services` — Cria um novo serviço.
- `GET /api/services/{id}` — Exibe um serviço.
- `PUT /api/services/{id}` — Atualiza um serviço.
- `DELETE /api/services/{id}` — Remove um serviço.
- `GET /api/services/search/name/{name}` — Busca serviços pelo nome.

### Experiências

- `GET /api/experiences` — Lista todas as experiências.
- `POST /api/experiences` — Cria uma nova experiência.
- `GET /api/experiences/{id}` — Exibe uma experiência.
- `PUT /api/experiences/{id}` — Atualiza uma experiência.
- `DELETE /api/experiences/{id}` — Remove uma experiência.
- `GET /api/experiences/user/{user_id}` — Lista experiências de um usuário.

### Publicações

- `GET /api/publications` — Lista todas as publicações.
- `POST /api/publications` — Cria uma nova publicação.
- `GET /api/publications/{id}` — Exibe uma publicação.
- `PUT /api/publications/{id}` — Atualiza uma publicação.
- `DELETE /api/publications/{id}` — Remove uma publicação.

### Serviços do Usuário

- `POST /api/service/user` — Associa serviços a um usuário.

### Recuperação de Senha

- `POST /api/forgot-password` — Solicita redefinição de senha.
- `POST /api/reset-password` — Redefine a senha.

> Algumas rotas requerem autenticação via token Sanctum.

---
