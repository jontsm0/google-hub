# ADR 0001 — Fundação do projeto Google Hub

- **Status**: Aceita
- **Data**: 2026-10-02

## Contexto

O projeto precisa iniciar com escopo controlado para viabilizar estudo prático e evolução segura de integrações Google.

## Decisão

1. Backend em **Python + FastAPI**.
2. Frontend em **React + TypeScript**.
3. MVP com **uma conta Google** e foco em **Drive, Calendar e Tasks**.
4. Uso do **9Drive como referência arquitetural**, sem dependência direta.
5. Entregas incrementais com documentação explícita de itens planejados x implementados.

## Consequências

- reduz complexidade inicial de autenticação e modelagem;
- acelera a curva de aprendizado em Python/FastAPI;
- mantém base modular para futuras integrações (Gmail, Docs, Sheets, Gemini, Keep, Chat, NotebookLM);
- exige disciplina de segurança para OAuth e tratamento de tokens.
