🚀 FastAPI + Airflow: Sistema Integrado de Monitoramento e Orquestração de Dados

Um sistema completo de microserviços para monitoramento, processamento de dados e orquestração com autenticação avançada, alertas em tempo real e escalabilidade nativa em containers.

📋 Índice
Visão Geral

Arquitetura do Sistema

Funcionalidades Principais

Tecnologias Utilizadas

🚀 Como Executar o Projeto

📡 API FastAPI - Endpoints

⚙️ Airflow - DAGs e Orquestração

🔐 Sistema de Segurança

⚠️ Sistema de Alertas

🛠️ Comandos Úteis

📊 Monitoramento e Métricas

📈 Próximos Passos

🎯 Habilidades Demonstradas

📄 Licença

Visão Geral
Este projeto implementa uma arquitetura moderna de microsserviços integrando FastAPI para APIs RESTful e Apache Airflow para orquestração de pipelines de dados. O sistema inclui autenticação JWT com 2FA, monitoramento automático, sistema de alertas em tempo real e processamento ETL escalável, tudo containerizado com Docker e orquestrado via Docker Compose.

Objetivo Principal: Demonstrar habilidades completas em backend, engenharia de dados, DevOps e segurança, aplicáveis a vagas de Engenheiro de Software, Engenheiro de Dados ou DevOps.

🏗️ Arquitetura do Sistema

https://imgur.com/a/Jp6YVlx

Fluxo de Dados:

API REST gerencia CRUD de embalagens com autenticação robusta

Airflow monitora a API periodicamente e executa pipelines ETL

Sistema de Alertas detecta falhas e notifica em tempo real

Volume compartilhado permite integração entre serviços

Dashboard consolida métricas e relatórios

✨ Funcionalidades Principais
🔐 Autenticação & Segurança
JWT com expiração configurável

Two-Factor Authentication (2FA) com QR Code

Hash de senhas com PBKDF2

Rate limiting em endpoints críticos

CORS configurado

📊 Processamento de Dados
Pipeline ETL completo com Pandas

Agregações e transformações em tempo real

Geração automática de relatórios

Armazenamento em formatos estruturados (JSON, CSV)

⚡ Monitoramento & Alertas
Health checks automáticos a cada 30 minutos

Monitoramento de API, banco e cache

Sistema de alertas com múltiplos níveis de severidade

Logs estruturados e centralizados

🔄 Orquestração
DAGs configuráveis no Airflow
usando terraform pra estruturar as permissoes , executar a estrutura do projeto
terraform init, terraform plan, terraform apply
https://terraformexecucao.s3.us-east-1.amazonaws.com/terraform.png


Agendamento flexível (cron expressions)

sistema rodando em docker e ambiente aws
https://arquivosprojeto.s3.us-east-1.amazonaws.com/dags+projeto.png

Execução distribuída com Celery

Retry automático em falhas

🛠️ Tecnologias Utilizadas
Categoria	Tecnologia	Versão	Finalidade
Backend	FastAPI	0.104.1	Framework API REST assíncrono
ORM	SQLAlchemy	2.0.23	Mapeamento objeto-relacional
Banco de Dados	PostgreSQL	13	Banco relacional principal
Cache	Redis	7	Sessões e cache de dados
Autenticação	PyOTP, python-jose	2.8.0	2FA e tokens JWT
Orquestração	Apache Airflow	2.7.3	Pipeline e agendamento
Processamento	Pandas	2.1.4	Transformação de dados
Container	Docker	3.8	Containerização
Orquestração	Docker Compose	3.8	Multi-container management
Cloud	AWS RDS	-	Banco de dados gerenciado
🚀 Como Executar o Projeto
Pré-requisitos
Docker e Docker Compose instalados

4GB RAM disponível

Python 3.9+ (para desenvolvimento local)

Passo a Passo
Clone o repositório

bash
git clone https://github.com/seu-usuario/fastapi-airflow-system.git
cd fastapi-airflow-system
Configure as variáveis de ambiente

bash
cp .env.example .env
# Edite o .env com suas configurações
Inicialize os serviços

bash
# 1. Suba os serviços base
docker-compose up -d redis airflow-postgres

# 2. Aguarde a inicialização
sleep 15

# 3. Inicialize o Airflow
docker-compose up -d airflow-init

# 4. Suba os serviços principais
docker-compose up -d airflow-webserver airflow-scheduler airflow-worker api

# 5. Verifique o status
docker-compose ps
Acesse as interfaces
sistema de api com sistema financeiro da empresa
http://localhost:8001/docs
http://localhost:8002/docs
http://localhost:8003/docs

FastAPI API: http://localhost:8004/docs

Airflow UI: http://localhost:8080 (usuário: , senha: )

API Docs (Swagger): http://localhost:8004/docs

Execute as DAGs iniciais

bash
# Listar DAGs disponíveis
docker-compose exec airflow-webserver airflow dags list

# Executar DAG de monitoramento
docker-compose exec airflow-webserver airflow dags trigger monitor_api
📡 API FastAPI - Endpoints Principais
🔐 Autenticação
Método	Endpoint	Descrição
POST	/login	Autenticação com JWT
POST	/2fa/enable	Ativa 2FA (retorna QR Code)
POST	/2fa/verify	Verifica código 2FA
📦 CRUD Embalagens
Método	Endpoint	Descrição
GET	/embalagens	Lista todas embalagens
GET	/embalagens/{id}	Busca embalagem específica
POST	/embalagens	Cria nova embalagem
PUT	/embalagens/{id}	Atualiza embalagem
DELETE	/embalagens/{id}	Remove embalagem
🩺 Saúde do Sistema
Método	Endpoint	Descrição
GET	/health	Status da API, DB e Redis
GET	/metrics	Métricas de performance
GET	/api/relatorios	Lista relatórios disponíveis
🔔 Webhooks
Método	Endpoint	Descrição
POST	/webhook/airflow	Aciona DAGs baseado em eventos
⚙️ Airflow - DAGs e Orquestração
DAGs Implementadas
monitor_api - A cada 30 minutos

Verifica saúde da API

Coleta métricas de performance

Registra em arquivo JSON para análise

process_data - Diariamente às 2h

Pipeline ETL completo

Extrai dados da API

Transforma com Pandas

Gera relatórios agregados

hello_airflow - DAG de exemplo

Demonstração de tarefas simples

Template para novas DAGs

Agendamento
python
# Exemplos de schedule_interval
'@daily'           # Diariamente à meia-noite
'@hourly'          # A cada hora
'*/30 * * * *'     # A cada 30 minutos
'0 2 * * *'        # Diariamente às 2h
'0 9-17 * * 1-5'   # Hora em hora, dias úteis
🔐 Sistema de Segurança
Camadas de Proteção
Autenticação: JWT + 2FA para admin

Autorização: RBAC baseado em roles

Validação: Pydantic para todos os inputs

Rate Limiting: Limite de requisições por usuário

CORS: Origens configuráveis

HTTPS Ready: Pronto para SSL/TLS

Implementação 2FA
python
# Geração de QR Code para app autenticador
@app.post("/2fa/enable")
def enable_2fa():
    secret = pyotp.random_base32()
    totp_url = pyotp.totp.TOTP(secret).provisioning_uri(
        name=user.username,
        issuer_name="API Embalagens"
    )
    # Retorna QR Code em base64
⚠️ Sistema de Alertas
Níveis de Severidade
python
SEVERITY_LEVELS = {
    'INFO': '🟢',      # Informação
    'WARNING': '🟡',   # Atenção necessária
    'ERROR': '🔴',     # Falha recuperável
    'CRITICAL': '⚫'   # Falha crítica
}
Alertas Configurados
DB_CONNECTION_LOST: Banco indisponível

API_HIGH_LATENCY: Latência > 5 segundos

AIRFLOW_DAG_FAILED: Falha em execução de DAG

LOW_DISK_SPACE: Espaço em disco < 10%

Canais de Notificação
Arquivo de log (alerts.log)

Email (SMTP configurável)

Slack Webhook (pronto para implementação)

Dashboard em tempo real

🛠️ Comandos Úteis
Docker & Airflow
bash
# Status dos containers
docker-compose ps

# Logs em tempo real
docker-compose logs -f api
docker-compose logs -f airflow-scheduler

# Acessar containers
docker-compose exec api bash
docker-compose exec airflow-webserver bash

# Gerenciar DAGs
docker-compose exec airflow-webserver airflow dags list
docker-compose exec airflow-webserver airflow dags pause monitor_api
docker-compose exec airflow-webserver airflow dags trigger process_data

# Backup do banco
docker-compose exec airflow-postgres pg_dump -U airflow airflow > backup.sql
Testes da API
bash
# Health check
curl http://localhost:8004/health

# Autenticação
curl -X POST http://localhost:8004/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"admin"}'

# Listar embalagens (com token)
curl http://localhost:8004/embalagens \
  -H "Authorization: Bearer SEU_TOKEN_JWT"
Monitoramento Avançado
bash
# Uso de recursos
docker stats

# Inspecionar rede
docker network inspect projeto_default

# Logs com filtro
docker-compose logs api | grep -i error
docker-compose logs --tail=100 --follow airflow-webserver
📊 Monitoramento e Métricas
Métricas Coletadas
json
{
  "timestamp": "2024-01-15T10:30:00Z",
  "api_status": "ok",
  "database_status": "connected",
  "cache_status": "connected",
  "response_time": 0.145,
  "active_users": 42,
  "embalagens_count": 1250
}
KPIs do Sistema
Métrica	Alvo	Monitoramento
Uptime API	> 99.9%	Health checks
Latência	< 200ms	Métricas contínuas
Taxa de erro	< 0.1%	Logs agregados
Sucesso DAGs	> 95%	Airflow metrics
Tempo resposta alertas	< 5min	Sistema de alertas
Freshness dos dados	< 15min	Timestamps
📈 Próximos Passos
🎯 Curto Prazo (1-2 semanas)
Implementar testes automatizados com Pytest

Adicionar documentação Swagger completa

Configurar CI/CD com GitHub Actions

Dashboard Grafana para métricas

🚀 Médio Prazo (1 mês)
Mais DAGs de processamento complexo

Cache Redis avançado com invalidation

Autenticação OAuth2 com providers externos

Ambientes staging/production separados

🌟 Longo Prazo (3+ meses)
Migração para Kubernetes

Streaming com Apache Kafka

Pipelines de machine learning

Arquitetura multi-tenancy

🎯 Habilidades Demonstradas
🏗️ Engenharia de Software
Arquitetura de microsserviços

Design de APIs RESTful com FastAPI

Padrões de projeto e clean code

Versionamento com Git

🗄️ Engenharia de Dados
Pipeline ETL com Airflow e Pandas

Processamento e agregação de dados

Quality assurance de dados

Orquestração complexa de workflows

☁️ DevOps & SRE
Containerização com Docker

Orquestração com Docker Compose

Monitoramento proativo e alertas

Infraestrutura como código

🔒 Segurança
Autenticação JWT e 2FA

Autorização baseada em roles

Rate limiting e CORS

Proteção de dados sensíveis

📊 Monitoramento & Observabilidade
Health checks automatizados

Métricas e logging estruturado

Sistema de alertas escalável

Dashboard de performance

🔄 Integração & APIs
Comunicação entre serviços

Webhooks e eventos

Volume compartilhado entre containers

API documentation automática

📄 Licença
Este projeto está licenciado sob a MIT License - veja o arquivo LICENSE para detalhes.

🤝 Contribuição
Contribuições são bem-vindas! Sinta-se à vontade para abrir issues e pull requests.

Fork o projeto

Crie sua feature branch (git checkout -b feature/AmazingFeature)

Commit suas mudanças (git commit -m 'Add some AmazingFeature')

Push para a branch (git push origin feature/AmazingFeature)

Abra um Pull Request

📞 Contato
Se você está interessado em uma demonstração ou tem oportunidades profissionais:

LinkedIn: Daniel Fonseca

GitHub: @seu-usuario

Portfólio: seu-portfolio.com

https://imgur.com/a/Jp6YVlx fotos sistema em produção




Nota: Este projeto está pronto para produção e demonstra competência técnica em múltiplas áreas relevantes para vagas de engenharia de software, dados e DevOps.
