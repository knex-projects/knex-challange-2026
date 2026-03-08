# Desafio Frontend — Case

Cada participante deve implementar **seu próprio frontend** consumindo a API do projeto. O intuito é que cada um construa a interface à sua maneira: stack, design e organização ficam por sua conta. O que unifica o case são os fluxos e as funcionalidades descritas abaixo.

---

## Visão geral

- **Autenticação:** tela padrão de **login** e **registro** (como cada um quiser estilizar).
- **Após o login:** duas perspectivas principais:
  1. **Visualização** — uma “landing” da loja (somente leitura), acessível apenas logado.
  2. **Gestão** — painel onde o usuário faz o CRUD de produtos (criar, listar, editar, excluir).

A API a ser consumida está documentada em: **`knex-frontend-api-to-consume/README.md`** (rotas, payloads, autenticação).

---

## O que implementar

### 1. Páginas de autenticação (públicas)

- **Registro**  
  - Formulário: nome, email, senha.  
  - Chamar `POST /auth/register`.  
  - Após sucesso, redirecionar para login ou já logar automaticamente, como preferir.

- **Login**  
  - Formulário: email, senha.  
  - Chamar `POST /auth/login`.  
  - Guardar o **token** retornado (ex.: localStorage/sessionStorage/contexto) e usá-lo no header `Authorization` em todas as requisições protegidas.

Rotas e layout (uma página, duas abas, etc.) ficam a seu critério.

---

### 2. Área logada — Duas “abas” ou seções

Acesso apenas para usuário autenticado. Se não houver token válido, redirecionar para login.

#### 2.1 Visualização (landing da loja)

- **Objetivo:** exibir os produtos da loja do usuário de forma **somente leitura**, como uma vitrine/landing.
- **Dados:** `GET /products` (com `Authorization: <token>`).
- **Exibição:** usar os dados retornados (nome, descrição, preço, imagem via `file.path`, `index` para ordenação, etc.) da forma que fizer mais sentido para você (grid, lista, cards, etc.).
- **URL da imagem:** `{baseUrl da API}/{file.path}` (ex.: `http://localhost:3000/abc-123_foto.jpg`).

Não é obrigatório ser uma “landing” no sentido de marketing; o importante é ser uma **visão de leitura** dos produtos, acessível só após login.

#### 2.2 Gestor de produtos (CRUD)

- **Listar:** mesma fonte `GET /products`, mas em uma interface de gestão (tabela, cards com ações, etc.).
- **Criar:**  
  - Primeiro: upload da imagem com `POST /files` (multipart/form-data, campo `file`).  
  - Depois: `POST /products` com `name`, `description`, `price`, `file_id` (UUID retornado em `/files`).
- **Atualizar:** `PUT /products/:product_id` com os campos que quiser alterar (`name`, `description`, `price`, `index` — todos opcionais).
- **Excluir:** `DELETE /products/:product_id`.

Fluxo de formulários (um modal, página separada, inline, etc.) e uso do campo `index` para ordenação ficam a seu critério.

---

## Resumo do que é esperado

| Item                         | Obrigatório |
|-----------------------------|-------------|
| Página de registro          | Sim         |
| Página de login             | Sim         |
| Área de visualização (leitura dos produtos, só logado) | Sim |
| Gestor: listar produtos     | Sim         |
| Gestor: criar produto       | Sim         |
| Gestor: atualizar produto   | Sim         |
| Gestor: excluir produto     | Sim         |
| Consumir a API documentada em `knex-frontend-api-to-consume` | Sim |

**Livre:** tecnologia (React, Vue, Svelte, Angular, vanilla, etc.), design, estrutura de pastas, roteamento e estilo visual. O objetivo é cada um implementar o frontend consumindo a API da própria forma.

---

## Dica rápida de fluxo

1. Registrar → Login → guardar token.
2. Em toda requisição a `/products` e `/files`, enviar `Authorization: <token>`.
3. Upload de imagem → obter `file_id` → criar produto com esse `file_id`.
4. Na visualização e no gestor, usar `GET /products` e exibir imagens com `{baseUrl}/{file.path}`.

Boa implementação.
