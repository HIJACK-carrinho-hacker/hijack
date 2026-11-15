# projeto_fcsc — HIJACK

Este diretório é o repositório `HIJACK-carrinho-hacker/hijack` (só documentação). Ele contém, ignorados pelo `.gitignore`, dois repositórios independentes clonados nas pastas abaixo:

- `web/` — `HIJACK-carrinho-hacker/hijack-web`: monorepo Bun (frontend React, backend Express+WS, pacote `protocol`). Leia `web/AGENTS.md`.
- `esp32/` — `HIJACK-carrinho-hacker/hijack-esp32`: firmware PlatformIO + Arduino para ESP32-WROOM-32 DevKit 38 pinos. Leia `esp32/AGENTS.md`.

Fonte de verdade da especificação: `web/docs/PLANO_IMPLEMENTACAO_HIJACK.md` (software) e `esp32/docs/especificacao/` (carrinho físico, versão de 10/11/2026 — prevalece sobre o plano mestre em pinos, direção proporcional, failsafe e topologia de 1 carrinho).
Estado atual e próximo passo: `web/docs/progress.md` (e `esp32/docs/progress.md`). Plano de bancada por módulo: `esp32/docs/proximos-passos-hardware.md`.
Decisões e premissas: `web/docs/assumptions.md`.
Estado em 14/11/2026: software e firmware validados com o ESP32 real; a bancada física (L298N, dois motores TT soltos e servo solto) foi energizada e testada com o `esp32dev_hw`; **nada foi testado no chassi nem no chão**. Rede da apresentação: hotspot do celular em 2,4 GHz (30 min sem apagão; a mesh da casa tinha apagões a cada ≈ 5 min). Lições da bancada em `esp32/docs/proximos-passos-hardware.md` (GND dedicado do ESP no USB, PWM máximo de 80 %, serial reinicia a placa). Em 15/11/2026 o 5 V do regulador chega ao ESP com queda de ≈ 0,5 V (4,99 V no regulador, 4,5 a 4,6 V nos pinos do ESP, sob carga) por contatos de protoboard e jumpers; a ligação será refeita com fios curtos e grossos e medida de novo.

Em 15/11/2026 o hacker ganhou dez ataques novos simulados no servidor (viés, travar direção, atraso, perda, replay, interceptar, quebra de senha, link derrubado, telemetria falsa e sequestro) e a 4ª defesa (criptografia simulada); o firmware não mudou. Decisões em `web/docs/assumptions.md` seção 7. O ESP32 está com o `esp32dev` (saídas desligadas) desde 15/11/2026.

Guia de uso para pessoas: `README.md` (raiz), `web/README.md`, `esp32/README.md`.

Regras gerais:
- Commit ao fim de cada passo verificado, no repositório afetado (`hijack`, `hijack-web` ou `hijack-esp32`). **Não fazer push** sem pedido explícito.
- Documentação e comunicação em português do Brasil.
- Nunca declarar hardware testado quando só houve simulação/compilação.

## Convenções do usuário

- **Sem comentários no código.** Explicações necessárias vão para `docs/`; comentários desnecessários não devem existir.
- **Usar o gerador oficial** de cada ferramenta para iniciar projetos/pacotes (`bun init`, `bun create vite`, `shadcn init/add`, `pio project init`...). Nunca criar a estrutura inteira à mão.
