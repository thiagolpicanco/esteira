# esteira

Repositório que centraliza a **estrutura, o contrato e a coordenação de agentes** que trabalham em múltiplos repositórios da PicancoTeam.

## O que tem aqui

- **`contrato/sdlc.md`** — Contrato de SDLC. Define o fluxo de desenvolvimento, as responsabilidades de cada agente (PO, Tech Lead, Desenvolvedor, Testador, Agilista) e como trabalhar com branches, PRs e tarefas do Paperclip. Mudanças neste arquivo são feitas por PR, sob responsabilidade do Agilista.
- **`AGENTS.md`** — Guia para agentes. Aponta para o contrato e define o que pode ou não entrar neste repositório.
- **Este `README.md`** — Você está lendo.

## Como é organizado

```
esteira/
├── contrato/
│   └── sdlc.md          # Fonte da verdade do contrato SDLC
├── AGENTS.md            # Instruções para agentes
└── README.md            # Este arquivo
```

## Quem trabalha aqui

- **PO, Tech Lead, Desenvolvedor, Testador, Agilista** — Todos os agentes do time PicancoTeam.
- O **Agilista** é o responsável por manter e atualizar o contrato (`contrato/sdlc.md`).

## Como trabalhar

As mesmas regras do contrato SDLC valem aqui:
- Crie uma branch `pic-<número>/<resumo>` a partir de `main`.
- Abra um PR com a mudança.
- Sem push direto em `main`. O merge é do Thiago.

Para entender todo o fluxo de desenvolvimento, leia `contrato/sdlc.md`.
