# Plano de Desenvolvimento — Google Hub

Documento de referência para orientar implementação, decisões técnicas e evolução do Google Hub.

## Sumário

- [1. Visão e escopo](#1-visão-e-escopo)
- [2. Princípios do projeto](#2-princípios-do-projeto)
- [3. Stack e arquitetura base](#3-stack-e-arquitetura-base)
- [4. Fases, marcos e entregas](#4-fases-marcos-e-entregas)
- [5. Decisões técnicas iniciais](#5-decisões-técnicas-iniciais)
- [6. Segurança e conformidade](#6-segurança-e-conformidade)
- [7. Critérios do MVP](#7-critérios-do-mvp)
- [8. Roadmap pós-MVP](#8-roadmap-pós-mvp)
- [9. Progresso e próximos passos](#9-progresso-e-próximos-passos)

## 1. Visão e escopo

O Google Hub será uma aplicação pessoal para centralizar informações de serviços Google em uma experiência única e contextual.

Escopo inicial do MVP:

- uma conta Google;
- integração com Drive, Calendar e Tasks;
- dashboard com visão unificada;
- organização por projetos e recursos relacionados.

Fora do MVP:

- replicar integralmente interfaces oficiais Google;
- integração profunda com serviços sem API pública estável;
- automações avançadas e orquestrações complexas.

## 2. Princípios do projeto

- **Hub, não cópia**: conectar contextos sem substituir os apps oficiais.
- **Escopos progressivos**: pedir permissões apenas quando necessário.
- **MVP incremental**: entregar valor em ciclos pequenos e verificáveis.
- **Segurança por padrão**: tratar tokens e credenciais como dados críticos.
- **Clareza de status**: separar explicitamente o que é implementado e o que é planejado.

## 3. Stack e arquitetura base

### Stack inicial

- Backend: Python 3.12+, FastAPI, Uvicorn
- Frontend: React, TypeScript
- Dados: SQLite local (evolução futura para banco gerenciado)
- Integrações: Google APIs + OAuth 2.0

### Arquitetura base

```text
Frontend React
      ↓ HTTP/JSON
Backend FastAPI
      ├── Autenticação/OAuth
      ├── Regras de negócio
      ├── Integrações Google
      └── Projetos/relacionamentos
             ├── SQLite local
             ├── Drive API
             ├── Calendar API
             └── Tasks API
```

## 4. Fases, marcos e entregas

| Fase | Objetivo | Entregáveis principais | Status |
|---|---|---|---|
| F0 | Fundação do repositório | Estrutura inicial, docs base, plano | Concluída |
| F1 | Base de backend | `GET /health`, setup local, dependências | Em andamento |
| F2 | Modelo de dados inicial | entidades de projeto e vínculo de recurso | Planejada |
| F3 | OAuth Google | início/callback, sessão e token seguro | Planejada |
| F4 | Integração Drive | listagem de arquivos recentes | Planejada |
| F5 | Integração Calendar | próximos eventos no dashboard | Planejada |
| F6 | Integração Tasks | tarefas pendentes no dashboard | Planejada |
| F7 | Dashboard e projetos | visão unificada e relacionamentos | Planejada |
| F8 | Qualidade e release inicial | testes, checklist de segurança, documentação | Planejada |

### Marco técnico mínimo (F1)

- ambiente virtual Python configurável;
- dependências iniciais documentadas;
- aplicação FastAPI executando localmente;
- endpoint de saúde respondendo:

```json
{
  "status": "ok"
}
```

## 5. Decisões técnicas iniciais

1. **Python/FastAPI no backend** para acelerar aprendizado e produtividade.
2. **React/TypeScript no frontend** para interface moderna e tipada.
3. **Uma conta Google no MVP** para reduzir complexidade inicial.
4. **9Drive como referência conceitual** (não dependência).
5. **Integração por API + links oficiais** quando a experiência nativa for mais adequada.

Registro formal da fundação: [0001-project-foundation](./decisions/0001-project-foundation.md).

## 6. Segurança e conformidade

- não versionar `.env` com dados reais;
- usar `.env.example` com placeholders seguros;
- isolar segredos e tokens de logs;
- aplicar mínimo privilégio em OAuth;
- revisar periodicamente scopes e permissões.

Variáveis sensíveis previstas para ambientes locais/produção:

```text
GOOGLE_CLIENT_ID
GOOGLE_CLIENT_SECRET
GOOGLE_REDIRECT_URI
TOKEN_ENCRYPTION_KEY
DATABASE_URL
SESSION_SECRET
```

## 7. Critérios do MVP

- [ ] Login Google funcional
- [ ] Permissões progressivas por serviço
- [ ] Arquivos recentes (Drive)
- [ ] Próximos eventos (Calendar)
- [ ] Tarefas pendentes (Tasks)
- [ ] Dashboard unificado
- [ ] CRUD de projetos
- [ ] Vínculos de recursos por projeto
- [ ] Links para abrir recursos no Google
- [ ] Documentação de setup atualizada

## 8. Roadmap pós-MVP

Prioridades previstas:

1. Gmail (contexto de comunicação)
2. Docs e Sheets (produção)
3. Gemini (resumos e assistência)
4. Keep e Chat (organização e colaboração)
5. NotebookLM (integração indireta por atalhos/fluxos)
6. Docker e deploy em Google Cloud

## 9. Progresso e próximos passos

### Concluído

- definição de proposta e posicionamento;
- escolha da stack inicial;
- documentação estruturante;
- scaffold inicial do repositório.

### Próximo passo imediato

Criar a primeira iteração funcional da API com:

```text
GET /health
```

Depois, iniciar autenticação Google e base de projetos.
