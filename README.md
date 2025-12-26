# fastapi-docker-rds
projeto em docker funcionando
FastAPI + Docker + RDS PostgreSQL

API REST construída com FastAPI, containerizada com Docker, conectada a banco de dados PostgreSQL hospedado na AWS RDS.
API construída com FastAPI, utilizando Amazon RDS para banco de dados persistente, Redis para cache de requisições frequentes, autenticação JWT com 2FA via TOTP, proteção contra força bruta com Rate Limiting e monitoramento através de Health Check. Toda a infraestrutura é orquestrada com Docker Compose para ambientes reproduzíveis."
 Estrutura do Projeto

/projeto
 ├── app/
 │    ├── main.py
 │    ├── models.py
 │    ├── schemas.py
 │    ├── database.py
 │    ├── auth.py
 ├── requirements.txt
 └── Dockerfile

Configuração do Banco de Dados

No código (database.py)

import os
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

DATABASE_URL = os.getenv("DATABASE_URL")
if not DATABASE_URL:
    raise ValueError("DATABASE_URL não foi definido!")

engine = create_engine(DATABASE_URL)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
Base = declarative_base()
No container (docker run)

docker run -d -p 8000:8000 \
  -e DATABASE_URL="postgresql+psycopg2://USUARIO:SENHA@ENDPOINT:5432/NOME_DB" \
  --name fastapi-container fastapi-app
  Dockerfile

FROM python:3.11

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY ./app ./app

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", 
🚀 Comandos Docker

Build da imagem

docker build --no-cache -t fastapi-app .

Rodar o container

docker run -d -p 8000:8000 \
  -e DATABASE_URL="postgresql+psycopg2://USUARIO:SENHA@ENDPOINT:5432/NOME_DB" \
  --name fastapi-container fastapi-app

Verificar containers

docker ps -a

Ver logs

docker logs fastapi-container

📑 Testes e Documentação da API

Swagger UI

Acesse:

http://localhost:8000/docs

Endpoints disponíveis

POST /login

GET /users

POST /users

GET /users/{user_id}

PUT /users/{user_id}

DELETE /users/{user_id}

Exemplos com curl

# Criar usuário
curl -X POST "http://localhost:8000/users" \
     -H "Content-Type: application/json" \
     -d '{"username":"daniel","password":"1234","role":"admin"}'

# Listar usuários
curl "http://localhost:8000/users"

# Buscar por ID
curl "http://localhost:8000/users/1"

# Atualizar usuário
curl -X PUT "http://localhost:8000/users/1" \
     -H "Content-Type: application/json" \
     -d '{"username":"daniel","password":"nova_senha","role":"admin"}'

# Deletar usuário
curl -X DELETE "http://localhost:8000/users/1"

🧠 Solução de Problemas Comuns

Not Found em / → rota raiz não existe, use /docs ou endpoints válidos.

ERR_EMPTY_RESPONSE → Uvicorn não iniciou, verifique CMD no Dockerfile e logs.

DATABASE_URL None → variável não passada, use -e DATABASE_URL=....

Tabela duplicada → mantenha apenas UserDB em models.py.

Conflito de nome de container → docker rm <nome> ou use outro nome.

🛠 Extras

Rebuild completo

docker stop fastapi-container
docker rm fastapi-container
docker build --no-cache -t fastapi-app .
docker run -d -p 8000:8000 -e DATABASE_URL="..." --name fastapi-container fastapi-app

Limpar containers parados

docker container prune

Organização recomendada

models.py → UserDB

schemas.py → UserCreate, UserOut

auth.py → autenticação

database.py → Base, engine, SessionLocal
