# Arquitetura — Google Hub

Este diretório concentra a documentação de arquitetura do projeto.

## Direção arquitetural inicial

- **Frontend React/TypeScript**: interface única para navegação por contexto.
- **Backend FastAPI**: ponto central de autenticação, orquestração e normalização.
- **Integrações Google**: consumo por APIs oficiais com OAuth 2.0.
- **Persistência local inicial**: banco local para metadados, vínculos e estado do hub.

## Fluxo base

1. Frontend autentica no backend.
2. Backend solicita ou reutiliza autorização Google.
3. Backend consulta APIs habilitadas.
4. Backend normaliza dados por contexto (projeto, tarefa, agenda).
5. Frontend apresenta visão unificada e links para ferramentas oficiais.

## Evolução esperada

- detalhamento de diagramas por domínio;
- padrões de integração por serviço Google;
- decisões de segurança, observabilidade e deployment.
