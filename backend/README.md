# Backend — Google Hub

Backend planejado em Python/FastAPI para orquestrar autenticação, integrações e agregação de dados.

## Estado atual

Implementação inicial com endpoint de saúde:

- `GET /health`

Resposta esperada:

```json
{
  "status": "ok"
}
```

## Setup local

```bash
cd backend
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
uvicorn app.main:app --reload
```

No Windows PowerShell:

```powershell
cd backend
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
copy .env.example .env
uvicorn app.main:app --reload
```

## Próximos incrementos planejados

- autenticação Google OAuth com escopos progressivos;
- camada de persistência para projetos e vínculos;
- integrações MVP com Drive, Calendar e Tasks.
