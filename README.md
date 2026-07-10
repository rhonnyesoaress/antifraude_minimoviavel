# Motor de Antifraude (POC 2)

Ambiente local do projeto, definido a partir do Stack Tecnológico já
aprovado com o time.

## Como subir o ambiente

```bash
docker compose up
```

Isso sobe:
- **Postgres** (`localhost:5432`) — user/senha/db: `antifraude` / `antifraude` / `antifraude_db`
- **Redis** (`localhost:6379`)

Os demais serviços (scoring, quarantine, notification, admin-panel) ainda
não têm código — os blocos deles já estão no `docker-compose.yml`, só
comentados. Conforme cada um for sendo desenvolvido, basta:

1. Colocar o código na pasta correspondente em `services/`
2. Adicionar um `Dockerfile` naquela pasta
3. Descomentar o bloco do serviço no `docker-compose.yml`

## Estrutura

```
.
├── docker-compose.yml
├── services/
│   ├── scoring-service/         # Python 3.12 + FastAPI
│   ├── quarantine-service/      # Python 3.12 + FastAPI (orquestrador SAGA)
│   ├── notification-service/    # Node.js + NestJS
│   └── admin-panel/             # React + TypeScript + Tailwind
```

## Stack (referência rápida)

| Serviço | Tecnologia | Papel |
|---|---|---|
| scoring-service | Python 3.12 + FastAPI | Risk scoring multifatorial |
| quarantine-service | Python 3.12 + FastAPI | Orquestrador da SAGA, circuit breaker, anti-corruption layer |
| notification-service | Node.js + NestJS | Webhooks e alertas |
| admin-panel | React + TS + Tailwind | Painel de leitura (CQRS) |
| Postgres | — | Event Store + Read Models |
| Redis | — | Cache de leitura + mensageria (Streams) + broker do Celery |
