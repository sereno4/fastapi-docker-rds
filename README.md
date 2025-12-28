🔐 API de Gestão com Autenticação Avançada
FastAPI + Docker + AWS RDS + Redis + 2FA

Uma API REST segura, escalável e monitorada para gestão de recursos, construída com princípios de segurança por design, alta disponibilidade e boas práticas de engenharia moderna.

Funcionalidades Principais
Autenticação JWT com criptografia pbkdf2_sha256
Autenticação em Dois Fatores (2FA) via TOTP (Google Authenticator, Authy)
Proteção contra ataques de força bruta com Rate Limiting (5 tentativas/minuto)
Cache inteligente com Redis para endpoints de alta frequência
Health Check completo (banco, cache, dependências)
CRUD completo com validação Pydantic e documentação automática
Infraestrutura como Código com Docker Compose
Conexão segura com AWS RDS (PostgreSQL gerenciado)

graph LR
  A[Cliente] -->|HTTPS| B(FastAPI Container)
  B --> C[AWS RDS PostgreSQL]
  B --> D[Redis Container]
  C -->|Dados persistentes| E[(Banco de Dados)]
  D -->|Cache de 60s| F[Respostas rápidas]

Banco de dados: AWS RDS (alta disponibilidade, backups automáticos)
Cache: Redis local (reduz carga no banco em 80%+)
API: Isolada em container, com restart automático
Segurança: Nenhuma credencial hardcoded — tudo via variáveis de ambiente

/fastapi-docker-rds
├── app/
│   ├── main.py             # Entrypoint da API
│   ├── models.py           # Modelos SQLAlchemy (UserDB, EmbalagemDB)
│   ├── schemas.py          # Esquemas Pydantic (UserCreate, EmbalagemOut)
│   ├── auth.py             # JWT, 2FA, Rate Limiting, Segurança
│   └── database.py         # Conexão com AWS RDS
├── requirements.txt        # Dependências Python
├── Dockerfile              # Build da imagem
├── docker-compose.yml      # Orquestração (Redis + API)
└── .env.example            # Template de variáveis sensíveis


Camada
Tecnologia
Benefício
Credenciais
pbkdf2_sha256

Resistente a rainbow tables
Sessão
JWT com expiração
Tokens curtos e revogáveis

Acesso
2FA com TOTP
Fator adicional para admin
Proteção
Rate Limiting
Bloqueia brute force

Conexão
AWS RDS + TLS
Dados em trânsito criptografados
Infra

Containers isolados
Sem vazamento de dependências
 Acesse a API
Swagger UI: http://localhost:8000/docs
✅ Escalável: Adicione mais containers da API sem tocar no banco
✅ Segura: Nenhuma senha no código, 2FA obrigatório para admin
✅ Observável: Health Check + logs estruturados
✅ Reproduzível: docker-compose up funciona em qualquer máquina

Contato
Construído com 💙 para demonstrar engenharia de software profissional, segura e escalável.

Pronto para contribuir em ambientes de alta performance e segurança crítica.

✅ Este projeto está 100% funcional e documentado — pronto para ser exibido em entrevistas técnicas.
"Sistema com monitoramento proativo: falhas em banco ou cache são registradas automaticamente para auditoria."
alerta logs 


