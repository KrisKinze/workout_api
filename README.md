# Workout API - Desafio de API com FastAPI

Este projeto é uma API RESTful para gerenciamento de atletas, categorias e centros de treinamento de uma competição de crossfit, desenvolvida como parte de um desafio da DIO. A API foi aprimorada com funcionalidades avançadas para torná-la mais robusta e profissional.

## Funcionalidades Implementadas

Além das funcionalidades básicas de CRUD (Criar, Ler, Atualizar, Deletar), o projeto inclui as seguintes melhorias:

*   **Filtros Avançados:** O endpoint de listagem de atletas permite a filtragem por `nome` e `cpf`.
*   **Respostas Customizadas:** A listagem de atletas retorna um conjunto de dados simplificado (`nome`, `categoria`, `centro_treinamento`) para melhor performance e segurança.
*   **Tratamento de Erros Específico:** A API captura erros de integridade do banco de dados (ex: CPF duplicado) e retorna uma mensagem clara com o status HTTP `303 See Other`.
*   **Paginação:** A listagem de atletas é paginada, permitindo que os clientes da API consumam os dados em partes (`limit` e `offset`).

## Stack Tecnológica

*   **Linguagem:** Python 3.11
*   **Framework:** FastAPI
*   **Banco de Dados:** PostgreSQL (gerenciado com Docker)
*   **ORM:** SQLAlchemy
*   **Migrations:** Alembic
*   **Validação de Dados:** Pydantic

## Como Executar o Projeto

### Pré-requisitos

*   Python 3.11 (recomenda-se o uso de `pyenv` para gerenciar versões)
*   Docker e Docker Compose

### 1. Configuração do Ambiente

Clone o repositório e navegue até a pasta do projeto.

```bash
# Crie e ative um ambiente virtual
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# Instale as dependências
pip install -r requirements.txt
```

### 2. Banco de Dados

Com o Docker em execução, suba o container do PostgreSQL:

```bash
docker-compose up -d
```

Aguarde alguns segundos para o banco de dados iniciar completamente. Em seguida, aplique as migrations para criar as tabelas:

```bash
alembic upgrade head
```

### 3. Executando a API

Para iniciar o servidor da API, execute:

```bash
uvicorn workout_api.main:app --reload
```

A API estará disponível em `http://127.0.0.1:8000`. A documentação interativa (Swagger UI) pode ser acessada em `http://127.0.0.1:8000/docs`.

## Como Testar as Novas Funcionalidades

Você pode usar a documentação interativa (`/docs`) para testar os endpoints.

#### Testando Filtros e Paginação

Acesse o endpoint `GET /atletas` e utilize os seguintes parâmetros:

*   `http://127.0.0.1:8000/atletas/?nome=Joao`
*   `http://127.0.0.1:8000/atletas/?cpf=12345678900`
*   `http://127.0.0.1:8000/atletas/?limit=5&offset=0`

#### Testando Resposta Customizada

Ao fazer uma requisição para `GET /atletas`, observe que a resposta contém apenas os campos `nome`, `categoria` e `centro_treinamento`.

#### Testando Tratamento de Erro

1.  Crie um atleta via `POST /atletas`.
2.  Tente criar um novo atleta com o mesmo CPF.
3.  Verifique se a API retorna o status `303 See Other` e a mensagem de erro correspondente.