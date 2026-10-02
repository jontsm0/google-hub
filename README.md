# Google Hub

> Um hub pessoal para centralizar serviços do ecossistema Google em uma única experiência.

## Sobre o projeto

O **Google Hub** é um projeto pessoal desenvolvido para reunir, em um único ambiente, informações e ações relacionadas aos principais serviços Google utilizados no dia a dia.

A proposta não é substituir aplicações como Google Drive, Google Calendar, Google Tasks, Gmail ou Gemini. O objetivo é criar uma camada de organização que conecte esses serviços, apresente seus dados em um contexto comum e facilite o acesso entre eles.

Por exemplo, um projeto pode reunir:

- eventos do Google Calendar;
- tarefas do Google Tasks;
- arquivos do Google Drive;
- documentos e planilhas;
- e-mails relacionados;
- resumos e sugestões gerados por inteligência artificial.

## Motivação

As ferramentas do ecossistema Google são poderosas, mas normalmente são utilizadas em ambientes separados. Mesmo quando existe integração entre elas, o usuário frequentemente precisa abrir várias abas, alternar entre diferentes interfaces e manter manualmente o contexto das informações.

Este projeto busca investigar como uma aplicação própria pode:

- centralizar informações de diferentes serviços;
- relacionar eventos, tarefas, arquivos e mensagens;
- criar uma visão orientada a projetos;
- reduzir a necessidade de alternar entre várias aplicações;
- utilizar APIs externas de forma segura;
- aplicar conhecimentos de Python, APIs, OAuth e desenvolvimento web.

## Relação com o 9Drive

O projeto utiliza o repositório [9Drive](https://github.com/zenhosta/9drive) como referência conceitual e arquitetural.

O 9Drive demonstra como criar uma camada intermediária entre o usuário e serviços externos, utilizando:

```text
Serviço externo
      ↓
OAuth e permissões
      ↓
Backend intermediário
      ↓
Banco de dados
      ↓
Interface própria
```

No 9Drive, essa ideia é aplicada à conexão de contas Google Drive e armazenamento S3.

Neste projeto, o mesmo conceito será estudado e adaptado para uma única conta Google conectada a vários serviços:

```text
Uma conta Google
      ↓
Permissões OAuth
      ↓
Backend do Google Hub
      ↓
Integrações com APIs Google
      ↓
Dashboard unificado
```

O 9Drive não será utilizado integralmente como base do produto. Ele será utilizado como estudo de caso para compreender:

- integração com serviços externos;
- gerenciamento de conexões;
- armazenamento seguro de tokens;
- organização modular do backend;
- sincronização de dados;
- criação de uma experiência unificada.

## Objetivos

### Objetivo geral

Construir um hub pessoal capaz de reunir serviços Google e organizá-los em torno de projetos, tarefas e atividades.

### Objetivos específicos

- Implementar autenticação com Google OAuth.
- Conectar uma conta Google ao sistema.
- Integrar inicialmente Google Drive, Google Calendar e Google Tasks.
- Exibir informações desses serviços em um dashboard.
- Criar projetos dentro do hub.
- Relacionar arquivos, eventos e tarefas.
- Permitir que o usuário abra os recursos nas aplicações oficiais do Google.
- Criar uma arquitetura preparada para futuras integrações.
- Aprender e aplicar Python em um projeto real.

## Escopo inicial

A primeira versão do projeto terá como foco:

```text
Autenticação Google
    ↓
Google Drive
    ↓
Google Calendar
    ↓
Google Tasks
    ↓
Dashboard
    ↓
Projetos e relacionamentos
```

### Funcionalidades planejadas para o MVP

- Login com Google.
- Conexão com a conta Google do usuário.
- Listagem de arquivos recentes do Google Drive.
- Listagem de próximos eventos do Google Calendar.
- Listagem de tarefas pendentes do Google Tasks.
- Dashboard com visão resumida.
- Criação de projetos.
- Associação de arquivos, eventos e tarefas a projetos.
- Links para abrir os recursos nas aplicações oficiais.

## Integrações futuras

Após o MVP, poderão ser estudadas as seguintes integrações:

- Gmail;
- Google Docs;
- Google Sheets;
- Google Keep;
- Google Chat;
- Gemini;
- NotebookLM;
- Google Workspace Add-ons.

Nem todos os serviços possuem o mesmo nível de integração. Algumas funcionalidades poderão ser implementadas através de APIs oficiais, enquanto outras poderão ser disponibilizadas apenas como links ou atalhos para as aplicações originais.

## Exemplo de uso

Imagine o projeto:

```text
Planejamento de viagem
```

O hub poderá reunir:

### Google Calendar

- voo de ida;
- reserva do hotel;
- passeio programado.

### Google Tasks

- confirmar reserva;
- comprar seguro;
- separar documentos.

### Google Drive

- passagens;
- comprovantes;
- roteiro;
- documentos pessoais.

### Gmail

- confirmação da companhia aérea;
- mensagens do hotel;
- comprovantes de reserva.

Em vez de procurar essas informações em vários aplicativos, o usuário poderá visualizar os recursos relacionados dentro do mesmo projeto.

## Arquitetura planejada

```text
┌──────────────────────────────┐
│        Frontend React        │
│ Dashboard, projetos e telas  │
└──────────────┬───────────────┘
               │ HTTP/JSON
               ▼
┌──────────────────────────────┐
│        Backend FastAPI       │
│ Regras de negócio e OAuth    │
└──────────────┬───────────────┘
               │
       ┌───────┴────────┐
       ▼                ▼
┌─────────────┐  ┌──────────────┐
│ Banco local │  │ Google APIs  │
│ SQLite      │  │ Drive        │
│             │  │ Calendar     │
│             │  │ Tasks        │
└─────────────┘  └──────────────┘
```

## Stack planejada

### Backend

- Python 3.12+
- FastAPI
- Uvicorn
- Pydantic
- SQLAlchemy
- Alembic
- pytest

### Integrações

- Google API Client para Python
- Google Auth
- OAuth 2.0
- APIs do Google Drive, Calendar e Tasks

### Frontend

- React
- TypeScript
- Vite
- React Router
- CSS ou Tailwind CSS

### Banco de dados

- SQLite no desenvolvimento
- PostgreSQL ou Cloud SQL em uma futura versão de produção

### Infraestrutura futura

- Docker
- Google Cloud Run
- Google Cloud SQL
- Google Secret Manager

## Estrutura inicial

```text
google-hub/
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── config.py
│   │   ├── database.py
│   │   ├── auth/
│   │   ├── integrations/
│   │   │   ├── drive.py
│   │   │   ├── calendar.py
│   │   │   └── tasks.py
│   │   ├── projects/
│   │   └── resources/
│   ├── tests/
│   ├── requirements.txt
│   └── .env.example
├── frontend/
│   ├── src/
│   │   ├── pages/
│   │   ├── components/
│   │   └── lib/
│   └── package.json
├── docs/
│   └── PLANO_DE_DESENVOLVIMENTO.md
├── .gitignore
└── README.md
```

## Como executar futuramente

O projeto ainda está em fase inicial de planejamento e desenvolvimento.

A execução planejada do backend será semelhante a:

```bash
cd backend

python -m venv .venv
source .venv/bin/activate

pip install -r requirements.txt

uvicorn app.main:app --reload
```

No Windows PowerShell:

```powershell
cd backend

python -m venv .venv
.venv\Scripts\Activate.ps1

pip install -r requirements.txt

uvicorn app.main:app --reload
```

O frontend será executado separadamente:

```bash
cd frontend
npm install
npm run dev
```

## Segurança

Como o projeto acessará dados pessoais do usuário, segurança será uma preocupação central.

As principais práticas planejadas são:

- utilizar OAuth 2.0;
- solicitar apenas as permissões necessárias;
- armazenar tokens de forma criptografada;
- nunca versionar arquivos `.env`;
- utilizar variáveis de ambiente;
- limitar o acesso aos endpoints protegidos;
- registrar erros sem expor tokens;
- utilizar HTTPS em produção;
- considerar Google Secret Manager no deploy;
- permitir que o usuário desconecte a conta Google;
- separar permissões por serviço quando possível.

## Status do projeto

```text
[x] Definição da ideia
[x] Estudo do 9Drive
[x] Definição da stack inicial
[ ] Criação da estrutura do projeto
[ ] Endpoint de saúde da API
[ ] Autenticação Google
[ ] Integração com Google Drive
[ ] Integração com Google Calendar
[ ] Integração com Google Tasks
[ ] Dashboard inicial
[ ] Projetos e relacionamentos
[ ] Testes automatizados
[ ] Deploy
```



## Licença

Este projeto é experimental e destinado a estudos, portfólio e aprendizado.

A licença será definida conforme a evolução do projeto.
