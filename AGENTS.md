# projeto_fcsc — HIJACK

Este diretório é o repositório `HIJACK-carrinho-hacker/hijack` (só documentação). Ele contém, ignorados pelo `.gitignore`, dois repositórios independentes clonados nas pastas abaixo:

- `web/` — `HIJACK-carrinho-hacker/hijack-web`: monorepo Bun (frontend React, backend Express+WS, pacote `protocol`). Leia `web/AGENTS.md`.
- `esp32/` — `HIJACK-carrinho-hacker/hijack-esp32`: firmware PlatformIO + Arduino para ESP32-WROOM-32 DevKit 38 pinos. Leia `esp32/AGENTS.md`.

Fonte de verdade da especificação: `web/docs/PLANO_IMPLEMENTACAO_HIJACK.md`.
Estado atual e próximo passo: `web/docs/progress.md` (e `esp32/docs/progress.md`).
Decisões e premissas: `web/docs/assumptions.md`.

Guia de uso para pessoas: `README.md` (raiz), `web/README.md`, `esp32/README.md`.

Regras gerais:
- Commit ao fim de cada passo verificado, no repositório afetado (`hijack`, `hijack-web` ou `hijack-esp32`). **Não fazer push** sem pedido explícito.
- Documentação e comunicação em português do Brasil.
- Nunca declarar hardware testado quando só houve simulação/compilação.

## Convenções do usuário

- **Sem comentários no código.** Explicações necessárias vão para `docs/`; comentários desnecessários não devem existir.
- **Usar o gerador oficial** de cada ferramenta para iniciar projetos/pacotes (`bun init`, `bun create vite`, `shadcn init/add`, `pio project init`...). Nunca criar a estrutura inteira à mão.
