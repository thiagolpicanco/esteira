# Instruções para Agentes — Repositório Esteira

## Escopo

O repositório **esteira** centraliza:

1. **Contrato SDLC** (`contrato/sdlc.md`) — Fonte da verdade para o fluxo de desenvolvimento da PicancoTeam. Define como os agentes trabalham em múltiplos repositórios.
2. **Coordenação de agentes** — Instruções, configurações e documentação do processo de SDLC.
3. **Documentação do fluxo** — README, AGENTS.md e arquivos que explicam como trabalhar no time.

## Referência: Contrato SDLC

Leia `contrato/sdlc.md` para entender:
- O fluxo de tarefas do Paperclip
- As responsabilidades de cada papel (PO, Tech Lead, Desenvolvedor, Testador, Agilista)
- Como trabalhar com branches, PRs e commits
- As verificações obrigatórias antes de abrir um PR
- O fluxo de status e aceite

## O que NÃO entra neste repositório

- **Código de produto** — Cada repositório (Sales-API, Smartcheck, SecondBrain, etc.) tem seu próprio código.
- **Segredos, tokens ou credenciais** — Nada de API keys, senhas ou dados sensíveis.
- **Arquivos gerados** — Caches, builds, outputs de scripts, arquivos temporários.
- **Configuração local de IDE/app** — `.vscode/`, `.idea/`, arquivos locais não commitam.
- **Mudanças que não são do escopo da tarefa** — Respeite o escopo escrito no Paperclip.

## Workflow para PRs neste repositório

Assim como os outros repositórios, este segue as regras do próprio contrato SDLC:

1. Crie uma branch `pic-<número>/<resumo>` a partir de `main`
2. Faça as mudanças (contrato, documentação, etc.)
3. Verifique que os links e caminhos de markdown são válidos
4. Verifique que não há segredo no diff (`git diff`)
5. Abra um PR com o título `PIC-<número>: <resumo>`
6. Não faça push direto em `main` — o merge é do Thiago

Para mais detalhes, consulte `contrato/sdlc.md`.

## Verificações antes de abrir um PR

- ✓ Links no markdown são válidos (arquivo existe, referência correta)
- ✓ Caminhos internos são relativos e corretos
- ✓ Sem API keys, tokens, passwords ou credenciais
- ✓ `git diff origin/main...HEAD` não mostra arquivo sensível
- ✓ Conteúdo em português e sem primeira pessoa

## Contato

Dúvidas sobre o contrato SDLC? Leia `contrato/sdlc.md`. Para ajustes no contrato, crie uma tarefa no Paperclip e o Agilista fica responsável.
