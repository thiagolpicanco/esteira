# Contrato de SDLC — PicancoTeam

**Nota:** Este arquivo é a fonte da verdade para o contrato de SDLC. Mudanças nele são feitas por PR neste repositório, sob responsabilidade do Agilista.

Este é o contrato que o time de SDLC (**PO**, **Tech Lead**, **Desenvolvedor**, **Testador** e **Agilista**) segue em toda tarefa de desenvolvimento. Ele vem da tarefa [PIC-1](https://github.com/thiagolpicanco) (plano) e foi generalizado na tarefa [PIC-4](https://github.com/thiagolpicanco) para valer em **vários repositórios**.

- **Repositórios:** o time trabalha em vários repositórios. O repositório de cada tarefa é o **workspace do projeto da tarefa** no Paperclip (seção 0). Nenhum repositório é o padrão.
- **SecondBrain** (`github.com/thiagolpicanco/secondbrain`) é a **base de conhecimento do Thiago, o cérebro do CEO**. Não é o repositório padrão do time. Serve de contexto e só recebe mudança quando a tarefa é do projeto SecondBrain.
- **Tracker:** tarefas do Paperclip. O "Done" do ticket é o status `done` da tarefa. Jira fica para depois.
- **Time:** PO, Tech Lead, Desenvolvedor, Testador e Agilista. O CEO faz a entrada dos pedidos e o roteamento, e sempre põe a tarefa no projeto certo.

| Papel | Responsabilidade |
| --- | --- |
| **PO** | Escreve e prioriza os tickets. No fim, aceita a entrega contra o critério de aceite e move para `done`. |
| **Tech Lead** | Refina o ticket com a abordagem técnica e quebra o que for grande. Faz a revisão de código. |
| **Desenvolvedor** | Implementa, roda as verificações e abre o PR/MR. |
| **Testador** | Testa o PR/MR contra o critério de aceite e as verificações da seção 4. |
| **Agilista** | Fora do caminho de cada ticket. Sob demanda: tickets parados, gargalos, ajustes neste contrato e retrospectiva. |

- **Merge:** quem faz é o Thiago. Nenhum agente faz merge, push na branch base ou force push.

## 0. Repositório da tarefa

Cada tarefa pertence a um projeto do Paperclip, e o workspace desse projeto define o repositório, o host e a branch base.

| Projeto | Repositório | Host | Branch base |
| --- | --- | --- | --- |
| Sales-API | `sales-api` (`~/Projects/sales-api`) | GitLab (Globo) | `master` |
| Smartcheck | `sales-smartcheck` (`~/Projects/sales-smartcheck`) | GitLab (Globo) | `main` |
| SecondBrain | `thiagolpicanco/secondbrain` | GitHub | `main` |
| Esteira | `thiagolpicanco/esteira` | GitHub | `main` |

Projeto novo entra nesta tabela pelo CEO. Regras:

- **Tarefa sem projeto, ou num projeto sem workspace, não começa.** Quem pegar comenta, devolve ao CEO e põe em `blocked`.
- A **branch base** é a branch padrão do repositório (`git symbolic-ref refs/remotes/origin/HEAD`). Neste contrato, "branch base" substitui o antigo `main`.
- **PR ou MR:** no GitHub é pull request, no GitLab é merge request. As regras da seção 5 valem para os dois.
- **Uma tarefa, um repositório.** Se a mudança toca mais de um repositório, o Tech Lead quebra em uma subtarefa por repositório, cada uma no seu projeto.

**Precedência.** O `AGENTS.md`/`CLAUDE.md` do repositório da tarefa manda em **conteúdo, arquitetura, testes e formato de commit**. Este contrato manda no **fluxo de git** (branch, PR/MR, sem push na branch base, sem merge) e no **fluxo do Paperclip**. Se o repositório mandar commitar direto na branch base, vale este contrato: trabalhe em branch.

## 1. Entrada

O **CEO gera as demandas** e consulta seu **SecondBrain** (vault Obsidian com todas as ideias e contexto) para preparar a entrada. Depois escolhe o projeto (e com ele o repositório) e passa ao **PO**. O PO escreve a tarefa com:

- **Objetivo** em uma ou duas frases.
- **Critério de aceite** verificável, em lista.
- **Escopo:** repositório, quais pastas ou arquivos podem mudar e o que fica de fora.

Depois o PO atribui ao **Tech Lead**, que comenta a abordagem técnica (arquivos, riscos, como verificar). Se o trabalho não cabe num PR/MR revisável, o Tech Lead quebra em subtarefas. Só então a tarefa vai para o **Desenvolvedor**.

Se faltar objetivo, critério de aceite, escopo ou projeto, ninguém adivinha. Quem achou a falta comenta a dúvida, devolve para o PO e põe a tarefa em `blocked`. Uma tarefa vira um PR/MR.

## 2. Branch

- O nome segue `pic-<número>/<resumo-curto>`, em minúsculas, sem acento e com hífen. Exemplo: `pic-12/corrige-validacao-cupom`. Se o repositório exigir outro padrão no `AGENTS.md` (por exemplo, `feature/...`), use o do repositório e mantenha `pic-<número>` no nome.
- A branch sai da branch base atualizada (`git fetch origin && git switch -c pic-12/... origin/<base>`).
- É uma branch por tarefa. Ela nunca é reaproveitada entre tarefas.
- Se a branch base andar durante a revisão, faça rebase na própria branch. O force push só é permitido na branch da tarefa, com `--force-with-lease`.

## 3. Commit

- A mensagem segue o padrão do repositório (veja o `git log` e o `AGENTS.md`). Se o repositório não define padrão, ela é curta, em português, na 3ª pessoa, e diz o quê e o porquê: `Corrige a validação do cupom expirado.`
- O rodapé leva a referência da tarefa: `Refs: PIC-12`.
- A linha `Co-Authored-By: <modelo do agente> <noreply@anthropic.com>` só entra se o repositório permitir. **O `sales-smartcheck` proíbe**; nesse caso, omita.
- O commit tem uma mudança lógica. Em geral, uma tarefa dá um ou poucos commits.
- Use `git add` só nos arquivos da tarefa. Nada de `git add -A` às cegas. Arquivo temporário, segredo, `target/`, `graphify-out/` ou configuração local de IDE/app não entram.

## 4. Verificações obrigatórias (antes de abrir o PR/MR)

O Desenvolvedor roda tudo isto na branch, e o Testador roda de novo.

### 4.1 Em todo repositório

1. **Regras do repositório:** leia o `AGENTS.md`/`CLAUDE.md` do repositório antes de codar e siga as skills que ele marca como obrigatórias (por exemplo, testes).
2. **Build e testes:** rode o build e os testes que o repositório define. Sem regressão em relação à branch base: teste que já falhava na base não bloqueia, mas teste novo quebrado bloqueia.
3. **Testes acompanham o código:** lógica nova ou alterada vem com teste. Teste existente não perde assertiva nem ganha `@Disabled`/skip.
4. **Diff limpo:** só os arquivos do escopo da tarefa, sem segredo, token ou arquivo gerado por engano. Confira com `git diff origin/<base>...HEAD --stat`.

### 4.2 Perfil por repositório

- **Sales-API (Java/Maven):** `mvn test` (o perfil de CI exclui `@Tag("integration")`). Se mexeu em `*IntegrationTest`, rode também a integração. Siga as convenções de teste e de exceção do `AGENTS.md` (por exemplo, `Mockito.spy`, exceções que estendem `SalesApiException`). Mudança grande ganha entrada no `changelog/`.
- **Smartcheck (Java/Quarkus/Maven):** `mvn test`. Mudança que toca artefato normativo do ADLC roda `python3 scripts/verify-adlc-governance.py`, e change de integração roda `python3 scripts/verify-tests-not-weakened.py`. A cobertura mínima é de 90%. Siga o gate "só implementar depois de 'pode implementar'" do `AGENTS.md`.
- **SecondBrain (vault Obsidian):**
  1. **Índice atualizado:** `python3 scripts/reindex.py` (use `--log "tipo|descrição|caminho"` quando couber). O `Índice.md` gerado entra no mesmo PR.
  2. **Saúde do vault sem regressão:** `python3 scripts/vault_health.py` rodado na branch e no `main`. O PR não pode trazer achado novo.
  3. **Scripts compilam:** se algum `.py` mudou, `python3 -m py_compile scripts/*.py`.
  4. **Travas de conteúdo:** corpo de nota existente em `Reunioes/` e `Caderno/` não muda (exceções do `AGENTS.md`), nada duplicado, referências como `[[wikilink]]`, prosa em português, sem primeira pessoa.
- **Esteira (markdown):** conteúdo é markdown puro. Links e caminhos internos válidos (verificar com ferramenta tipo `markdown-link-check`). Sem segredo, token ou credencial. Toda mudança em `contrato/sdlc.md` é feita por PR do Agilista.
- **Repositório sem perfil aqui:** use os comandos de build e teste do `AGENTS.md`/`README` do repositório. Se não houver, o Tech Lead define na abordagem técnica antes do Desenvolvedor começar.

Se alguma verificação falhar e o Desenvolvedor não conseguir corrigir dentro do escopo, ele não abre o PR/MR. Ele comenta o problema e põe a tarefa em `blocked` para o Tech Lead.

## 5. Pull request / merge request

- **Título:** `PIC-<número>: <frase do commit principal>`.
- **Base:** a branch base do repositório. **Estado:** pronto para revisão. Não abra como draft para a revisão.
- **Descrição:** use o template do repositório, se existir. Se não existir, use este modelo:

  ```markdown
  ## O quê
  <uma ou duas frases>

  ## Por quê
  Tarefa: PIC-<número> — <link da tarefa no Paperclip>

  ## Critério de aceite
  - [x] <critério 1, copiado da tarefa>
  - [x] <critério 2>

  ## Verificações
  - [x] build e testes do repositório passando, sem regressão
  - [x] testes novos/alterados para a lógica mudada
  - [x] verificações do perfil do repositório (seção 4.2)
  - [x] diff só com arquivos do escopo

  ## Fora do escopo / riscos
  <o que ficou de fora ou o que o Testador e o Tech Lead devem olhar com atenção>
  ```

- O link do PR/MR vai num comentário da tarefa.

## 6. Teste, revisão e aceite

Ninguém reimplementa nem faz a própria versão da mudança. Cada etapa termina com um comentário na tarefa dizendo o que conferiu e o resultado.

**Teste (Testador):**

1. O diff atende a cada critério de aceite?
2. As verificações da seção 4 (geral e perfil do repositório) passam quando ele roda na branch?

Se passar, atribui ao Tech Lead. Se não, devolve ao Desenvolvedor com a lista do que falhou.

**Revisão de código (Tech Lead):**

1. A mudança segue a abordagem combinada e não tem problema de qualidade?
2. As convenções de branch, commit e PR/MR estão certas?
3. A mudança segue o `AGENTS.md` do repositório (arquitetura, testes, conteúdo)?

- **Aprovado:** deixa review de aprovação no PR/MR (ou comentário "Aprovado", se a ferramenta não permitir) e atribui ao PO.
- **Mudanças pedidas:** comentários no PR/MR apontando arquivo e item que falhou, e devolve ao Desenvolvedor.

**Aceite (PO):** confere a entrega contra o critério de aceite. Se aceitar, move a tarefa para `done`. Se não, devolve ao Desenvolvedor dizendo qual critério não foi atendido.

O limite é **três rodadas** de devolução ao Desenvolvedor, somando Testador, Tech Lead e PO. Se a terceira ainda não passar, a tarefa vai para `blocked`, atribuída ao CEO, com o resumo do impasse, e o Agilista é chamado para olhar o gargalo. O limite existe para segurar custo.

## 7. Fluxo de status no Paperclip

| De | Para | Quem | Quando |
| --- | --- | --- | --- |
| `todo` | `todo` | PO → Tech Lead | Tarefa escrita. Atribui ao Tech Lead para refinar. |
| `todo` | `todo` | Tech Lead → Desenvolvedor | Abordagem comentada. Atribui ao Desenvolvedor. |
| `todo` | `in_progress` | Desenvolvedor | Pega a tarefa (checkout). |
| `in_progress` | `in_review` | Desenvolvedor | PR/MR aberto e verificações passando. Atribui ao Testador e comenta o link. |
| `in_review` | `in_review` | Testador → Tech Lead | Teste passou. |
| `in_review` | `in_review` | Tech Lead → PO | Código aprovado. |
| `in_review` | `in_progress` | Testador, Tech Lead ou PO | Mudanças pedidas. Atribui ao Desenvolvedor. |
| `in_review` | `done` | PO | Entrega aceita. O PR/MR fica aberto e pronto para merge. |
| qualquer | `blocked` | quem travou | Falta informação, projeto/workspace, verificação impossível ou três rodadas sem aprovação. Diz o que destrava e para quem. |

"Done" quer dizer que o **PR/MR está aberto, testado, revisado, aceito pelo PO e pronto para merge**. O merge é do Thiago. Se ele pedir mudança depois do `done`, o PO reabre a tarefa (ou cria uma nova) e o ciclo recomeça.

Toda mudança de status leva um comentário curto em português dizendo o que mudou e qual é o próximo passo.

**Agilista:** não entra no fluxo de cada tarefa. É chamado quando uma tarefa fica parada, quando bate o limite de rodadas ou no fim de um piloto, e propõe ajustes a este contrato. Ajustes no contrato passam pelo CEO.

## 8. O que nenhum agente faz

- Merge de PR/MR, push na branch base, force push fora da própria branch, reescrever o histórico da branch base.
- Mexer em CI/CD (`.gitlab-ci.yml`, GitHub Actions), em configuração do GitHub/GitLab ou em segredos.
- Trabalhar fora do escopo escrito na tarefa ou fora do repositório do projeto da tarefa. Se aparecer algo fora do escopo, vira comentário para o CEO, não commit.
- Mudar o SecondBrain numa tarefa que não é do projeto SecondBrain. Consultar para ter contexto pode.
- Contratar agente ou mudar permissão.
- Pular uma etapa do fluxo da seção 7 (por exemplo, o Desenvolvedor mover para `done`).

## 9. Custo

Todos os agentes rodam só quando recebem tarefa, sem heartbeat agendado. Só o Tech Lead usa o Sonnet 5.5; os outros usam o Haiku 4.5. Uma tarefa passa por umas 5 ou 6 execuções de agente. As tarefas são pequenas, com um PR/MR cada, e ninguém reescreve o trabalho do outro. São no máximo três rodadas de devolução. Se uma tarefa pede investigação longa e aberta, o Tech Lead quebra antes de alguém gastar tokens nela.
