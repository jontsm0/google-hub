# Plano de Desenvolvimento — Google Hub

Este documento organiza as etapas de estudo, planejamento, implementação e evolução do Google Hub.

## 1. Visão do projeto

O Google Hub será uma aplicação pessoal que utiliza uma única conta Google para centralizar informações de diferentes serviços, inicialmente Google Drive, Google Calendar e Google Tasks.

A aplicação não pretende substituir os produtos oficiais do Google. Ela funcionará como uma camada de organização, relacionamento e acesso aos recursos existentes.

```text
Google Drive + Google Calendar + Google Tasks
                    ↓
            Backend do Google Hub
                    ↓
            Dashboard e projetos
```

## 2. Inspiração no 9Drive

O 9Drive será utilizado como estudo de caso para compreender como uma aplicação pode conectar-se a serviços externos e criar uma interface própria.

### Conceitos aproveitados

- autenticação OAuth;
- gerenciamento de conexões externas;
- armazenamento seguro de tokens;
- backend intermediário;
- organização modular;
- integração com APIs externas;
- sincronização de dados;
- dashboard unificado.

### Conceitos que não fazem parte do MVP

- múltiplas contas Google Drive;
- roteamento de uploads;
- distribuição de quota;
- integração S3;
- seleção entre diferentes provedores de armazenamento.

O objetivo é aproveitar a ideia arquitetural de um hub, e não reproduzir integralmente o 9Drive.

## 3. Stack planejada

### Backend

- Python 3.12+
- FastAPI
- Uvicorn
- Pydantic
- SQLAlchemy 2
- Alembic
- pytest

### Frontend

- React
- TypeScript
- Vite
- React Router
- CSS ou Tailwind CSS

### Integrações

- Google Drive API
- Google Calendar API
- Google Tasks API
- OAuth 2.0

### Banco de dados

- SQLite durante o desenvolvimento;
- PostgreSQL ou Cloud SQL em uma futura versão de produção.

### Infraestrutura futura

- Docker;
- Google Cloud Run;
- Google Cloud SQL;
- Google Secret Manager.

## 4. Arquitetura inicial

```text
Frontend React
      ↓ HTTP/JSON
Backend FastAPI
      ├── Autenticação e OAuth
      ├── Regras de negócio
      ├── Integrações Google
      └── Projetos e relacionamentos
             ├── SQLite
             ├── Google Drive API
             ├── Google Calendar API
             └── Google Tasks API
```

O frontend não deverá chamar diretamente as APIs Google. O backend será responsável por autenticação, tokens, regras de negócio, normalização das respostas e controle de erros.

## 5. Fases de desenvolvimento

### Fase 0 — Preparação

- Criar o repositório.
- Estudar o README e a estrutura do 9Drive.
- Definir o escopo do MVP.
- Criar um projeto no Google Cloud.
- Estudar Python, FastAPI, APIs REST e OAuth 2.0.

### Fase 1 — Backend básico

- Criar ambiente virtual Python.
- Instalar FastAPI e Uvicorn.
- Criar `app/main.py`.
- Criar o endpoint `GET /health`.
- Configurar variáveis de ambiente.
- Validar a API através do Swagger.

Resposta esperada:

```json
{
  "status": "ok"
}
```

### Fase 2 — Banco de dados

- Configurar SQLite.
- Criar conexão com SQLAlchemy.
- Configurar Alembic.
- Criar os modelos iniciais.
- Criar migrations.
- Implementar o CRUD de projetos.

Modelos iniciais:

```text
User
Project
ExternalResource
ProjectResource
```

Endpoints iniciais:

```text
GET  /api/projects
POST /api/projects
GET  /api/projects/{project_id}
PATCH /api/projects/{project_id}
DELETE /api/projects/{project_id}
```

### Fase 3 — Autenticação Google

- Criar credenciais OAuth no Google Cloud.
- Configurar as URLs de redirecionamento.
- Implementar `GET /auth/google/start`.
- Implementar `GET /auth/google/callback`.
- Trocar o código de autorização por tokens.
- Armazenar tokens de forma segura.
- Implementar `GET /auth/me`.
- Implementar logout e desconexão.

Começar com os escopos básicos:

```text
openid
email
profile
```

As permissões de Drive, Calendar e Tasks devem ser solicitadas progressivamente.

### Fase 4 — Google Drive

- Criar cliente autenticado do Drive.
- Listar arquivos recentes.
- Filtrar itens da lixeira.
- Normalizar nome, tipo, data e link.
- Exibir os dados no frontend.
- Adicionar ação para abrir o arquivo no Drive.

Endpoint:

```text
GET /api/drive/recent
```

O hub deve armazenar referências e metadados, não duplicar os arquivos do usuário.

### Fase 5 — Google Calendar

- Criar cliente autenticado do Calendar.
- Buscar eventos futuros.
- Definir o intervalo de consulta.
- Ordenar eventos por data.
- Normalizar os dados.
- Exibir os próximos eventos no dashboard.
- Adicionar ação para abrir no Google Calendar.

Endpoint:

```text
GET /api/calendar/events
```

### Fase 6 — Google Tasks

- Listar listas de tarefas.
- Selecionar a lista principal.
- Buscar tarefas pendentes.
- Normalizar título, status e vencimento.
- Exibir tarefas no dashboard.
- Permitir associação com projetos.

Endpoints:

```text
GET /api/tasks/lists
GET /api/tasks
```

### Fase 7 — Dashboard

Criar uma visão inicial com:

```text
Resumo do dia
Próximos eventos
Tarefas pendentes
Arquivos recentes
Projetos ativos
Atalhos para os serviços Google
```

O objetivo é que o usuário consiga entender suas atividades sem abrir imediatamente várias aplicações.

### Fase 8 — Projetos e relacionamentos

Criar o principal diferencial do Google Hub: organizar recursos de serviços diferentes dentro de um projeto.

Exemplo:

```text
Projeto: Estudos de Python
├── Evento do Calendar
├── Tarefa do Tasks
├── Arquivo do Drive
└── Documento relacionado
```

Funcionalidades:

- criar projeto;
- editar projeto;
- excluir projeto;
- adicionar recurso;
- remover recurso;
- abrir recurso externo;
- listar atividades relacionadas.

### Fase 9 — Sincronização

Inicialmente, a sincronização será feita sob demanda quando o dashboard for aberto.

Posteriormente, poderá ser criado um processo em segundo plano para atualizar dados periodicamente.

Registros futuros:

```text
SyncJob
SyncCursor
SyncError
```

### Fase 10 — Gmail

Após a conclusão do MVP, adicionar uma integração inicial com Gmail para:

- listar mensagens importantes ou recentes;
- associar conversas a projetos;
- criar tarefas a partir de e-mails;
- abrir conversas no Gmail;
- futuramente gerar resumos.

Essa fase exige atenção especial por envolver dados altamente sensíveis.

### Fase 11 — Gemini

Utilizar IA somente depois que os dados do hub estiverem organizados.

Possíveis usos:

- resumir um projeto;
- sugerir próximos passos;
- transformar e-mail em tarefa;
- resumir eventos da semana;
- gerar checklists;
- identificar pendências.

O usuário deverá saber quais dados serão enviados para análise.

### Fase 12 — Testes

Criar testes unitários e de integração para:

- autenticação;
- normalização de respostas;
- projetos;
- relacionamentos;
- autorização;
- tokens expirados;
- respostas simuladas das APIs Google.

Ferramentas:

```text
pytest
pytest-asyncio
httpx
unittest.mock
```

### Fase 13 — Segurança

- Nunca versionar arquivos `.env`.
- Utilizar `.env.example`.
- Criptografar tokens.
- Não exibir tokens nos logs.
- Validar dados de entrada.
- Proteger endpoints privados.
- Solicitar o mínimo de escopos possível.
- Implementar desconexão da conta.
- Utilizar HTTPS em produção.
- Usar Secret Manager no deploy.

Variáveis sensíveis esperadas:

```text
GOOGLE_CLIENT_ID
GOOGLE_CLIENT_SECRET
TOKEN_ENCRYPTION_KEY
DATABASE_URL
SESSION_SECRET
GEMINI_API_KEY
```

### Fase 14 — Docker

Depois da execução local funcionar:

- criar Dockerfile do backend;
- criar Dockerfile do frontend;
- criar `docker-compose.yml`;
- padronizar o ambiente de desenvolvimento;
- preparar o projeto para deploy.

### Fase 15 — Deploy

Arquitetura futura:

```text
Frontend → Cloud Run
Backend  → Cloud Run
Banco    → Cloud SQL
Segredos → Secret Manager
```

Etapas:

- criar imagens Docker;
- publicar no Artifact Registry;
- configurar Cloud Run;
- configurar o banco de produção;
- cadastrar secrets;
- atualizar as URLs OAuth;
- configurar domínio e HTTPS;
- validar logs e erros.

## 6. Critérios de conclusão do MVP

```text
[ ] Login com Google
[ ] Autorização dos serviços necessários
[ ] Arquivos recentes do Drive
[ ] Próximos eventos do Calendar
[ ] Tarefas pendentes do Tasks
[ ] Criação de projetos
[ ] Associação de arquivo ao projeto
[ ] Associação de evento ao projeto
[ ] Associação de tarefa ao projeto
[ ] Links para abrir recursos no Google
[ ] Dashboard funcional
[ ] Testes básicos
[ ] Documentação de instalação
```

## 7. Estratégia de estudos

A sequência recomendada é:

```text
1. Python básico
2. Módulos e pacotes
3. Tipagem
4. Ambientes virtuais
5. HTTP e JSON
6. FastAPI
7. Pydantic
8. SQLAlchemy
9. Alembic
10. OAuth 2.0
11. APIs externas
12. React e TypeScript
13. Testes
14. Docker
15. Google Cloud
```

Para cada tema:

1. estudar a teoria;
2. criar um exemplo pequeno;
3. aplicar no Google Hub;
4. documentar o aprendizado;
5. criar um commit específico.

## 8. Estratégia de commits

Usar mensagens claras:

```text
chore: initialize backend project
feat: add health endpoint
feat: create project model
feat: add Google OAuth flow
feat: integrate Google Drive
feat: integrate Google Calendar
feat: integrate Google Tasks
feat: create dashboard
feat: associate resources with projects
test: add project service tests
docs: update setup instructions
```

## 9. Critério de sucesso

O projeto será bem-sucedido quando demonstrar o seguinte fluxo:

```text
Uma conta Google
        ↓
Vários serviços conectados
        ↓
Dados normalizados
        ↓
Projetos e relacionamentos
        ↓
Dashboard único
```

O foco não é criar uma cópia dos produtos Google, mas demonstrar como diferentes serviços podem ser conectados por uma aplicação própria e apresentados em um contexto mais organizado.

## 10. Progresso

### Outubro de 2026

- [x] Definição da proposta do Google Hub.
- [x] Estudo inicial do 9Drive.
- [x] Identificação do padrão de hub e integrações.
- [x] Decisão de trabalhar com uma única conta Google.
- [x] Escolha inicial de Python e FastAPI.
- [x] Criação do repositório.
- [x] Definição do MVP.
- [ ] Estrutura inicial do backend.
- [ ] Estudo prático de FastAPI.
- [ ] Estudo prático de OAuth 2.0.
- [ ] Configuração do Google Cloud.

### Próximo objetivo

Criar a primeira API FastAPI com:

```text
GET /health
GET /api/projects
POST /api/projects
```
