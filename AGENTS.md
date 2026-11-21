# projeto_fcsc — HIJACK

Este diretório é o repositório `HIJACK-carrinho-hacker/hijack` (só documentação). Ele contém, ignorados pelo `.gitignore`, dois repositórios independentes clonados nas pastas abaixo:

- `web/` — `HIJACK-carrinho-hacker/hijack-web`: monorepo Bun (frontend React, backend Express+WS, pacote `protocol`). Leia `web/AGENTS.md`.
- `esp32/` — `HIJACK-carrinho-hacker/hijack-esp32`: firmware PlatformIO + Arduino para ESP32-WROOM-32 DevKit 38 pinos. Leia `esp32/AGENTS.md`.

Fonte de verdade da especificação: `web/docs/PLANO_IMPLEMENTACAO_HIJACK.md` (software) e `esp32/docs/especificacao/` (carrinho físico, versão de 10/11/2026 — prevalece sobre o plano mestre em pinos, direção proporcional, failsafe e topologia de 1 carrinho).
Estado atual e próximo passo: `web/docs/progress.md` (e `esp32/docs/progress.md`). Plano de bancada por módulo: `esp32/docs/proximos-passos-hardware.md`.
Decisões e premissas: `web/docs/assumptions.md`.
Estado em 14/11/2026: software e firmware validados com o ESP32 real; a bancada física (L298N, dois motores TT soltos e servo solto) foi energizada e testada com o `esp32dev_hw`; **nada foi testado no chassi nem no chão**. Em 17/11/2026 o usuário informou que todas as peças 3D já foram impressas; a montagem ainda não foi registrada. Rede da apresentação: hotspot do celular em 2,4 GHz (30 min sem apagão; a mesh da casa tinha apagões a cada ≈ 5 min). Lições da bancada em `esp32/docs/proximos-passos-hardware.md` (GND dedicado do ESP no USB, PWM máximo de 80 %, serial reinicia a placa). Em 15/11/2026 o 5 V do regulador chega ao ESP com queda de ≈ 0,5 V (4,99 V no regulador, 4,5 a 4,6 V nos pinos do ESP, sob carga) por contatos de protoboard e jumpers; a ligação será refeita com fios curtos e grossos e medida de novo.

Em 15/11/2026 o hacker ganhou dez ataques novos simulados no servidor (viés, travar direção, atraso, perda, replay, interceptar, quebra de senha, link derrubado, telemetria falsa e sequestro) e a 4ª defesa (criptografia simulada); o firmware não mudou. Decisões em `web/docs/assumptions.md` seção 7. Validados no ESP32 real com as saídas desligadas (23 de 23 verificações pelo protocolo, serial sem acionar pinos). Depois, na bancada com motores e servo soltos e o `esp32dev_hw`, 18/18 (observação física parcial). Também houve um jogo de ataque e defesa com os motores (registro em `web/docs/progress.md`). O ESP32 está com o `esp32dev` (saídas desligadas) desde as 19:30 de 15/11/2026; o `esp32dev_hw` só é gravado para testes com motores, com a fonte desligada. A perda de pacotes passou a ser em rajadas de 0,5 s. Para o Playwright, subir o servidor à mão com os carros simulados (`web/docs/testing.md`).

Em 17/11/2026 foram adicionadas informações educativas aos 12 controles de ataque e às 4 defesas: ícone de informação abre por mouse, toque ou teclado, com textos **Na vida real** e **No HIJACK**, sem instruções de execução de ataques. Implementação e limites em `web/docs/assumptions.md` seção 8. Feature no commit `18ffae7` do `hijack-web`, enviado para `origin/main` a pedido do usuário. Lint, typecheck, build e 343 testes passaram; Playwright direcionado (`security-info.spec.ts` e `novos-ataques.spec.ts`) 16/16. Em 17/11/2026, mais tarde, a suíte completa de Playwright foi executada até o fim com os carros simulados: **57 aprovados e 1 pulado** (WASD no perfil mobile), desktop e Pixel 7 emulado; lint, typecheck, build e 343 testes passaram de novo. Nenhuma mudança no servidor, protocolo ou firmware, nem nova validação de hardware. O usuário pediu sugestões de novas defesas; a conversa ficou pendente, sem escolha nem implementação.

Em 17/11/2026 foi escrito o **relatório final** em `relatorio/` (LaTeX `abntex2`, citações autor-data com `abntex2cite`, 31 páginas): contexto de ataques recentes com e sem IA (GTG-1002, LiteLLM, PHP2Shell/GitLab/Node.js, Binance, supermercados europeus, Porto de Santos e, desde 18/11/2026, Java4Shell), metodologia, o jogo, detalhamento técnico (componentes, ligações e cálculo do PWM, firmware, protocolo ESP32 ↔ servidor), resultados e limitações. Passou por revisão de outro agente e as correções foram aplicadas. Compilação no `README.md` da raiz. Regras do relatório: só o PDF vai para o professor, então **não citar arquivos internos** (`docs/`, `.md`); toda afirmação de notícia precisa estar na fonte citada no mesmo trecho, preferindo veículos grandes e, quando houver, brasileiros em português; as imagens em `figuras/` são renderizações do modelo 3D no Autodesk Fusion, não fotos; não declarar como testado no chão o que só foi testado em bancada. Uma cópia do PDF foi posta em `~/Downloads/HIJACK - Relatório final.pdf`. Os três repositórios foram enviados para `origin/main` a pedido do usuário em 17/11/2026.

Em 18/11/2026 foram feitos os **slides da apresentação** em `apresentacao/` (Beamer metropolis, 16:9, 19 páginas para 15 minutos): capa própria com integrantes, importância da cibersegurança, casos condensados, ODS 4, 9 e 16, o jogo, funcionamento, resultados e conclusão, cada quadro com pouco texto e uma caixa "ideia" no rodapé para guiar a fala. Imagens externas só do Wikimedia Commons, com autor e licença no slide; o Commons limita downloads seguidos (HTTP 429), e `Special:FilePath/<arquivo>?width=500` funcionou quando as miniaturas falharam. Cópia em `~/Downloads/HIJACK - Apresentação.pdf`.

Em 20/11/2026, a pedido do usuário, o analógico (piloto e sequestro) passou a manter o sentido da tração quando vai todo para o lado com o carrinho andando (antes o motor desligava e o carrinho parava). Só a tela mudou (commit `bbcfaea` do `hijack-web`, decisão em `web/docs/assumptions.md` seção 9); não testado no chão.

Guia de uso para pessoas: `README.md` (raiz), `web/README.md`, `esp32/README.md`. Roteiro didático: `web/docs/demo-script.md`.

Regras gerais:
- Commit ao fim de cada passo verificado, no repositório afetado (`hijack`, `hijack-web` ou `hijack-esp32`). **Não fazer push** sem pedido explícito.
- Documentação e comunicação em português do Brasil.
- Nunca declarar hardware testado quando só houve simulação/compilação.

## Convenções do usuário

- **Sem comentários no código.** Explicações necessárias vão para `docs/`; comentários desnecessários não devem existir.
- **Usar o gerador oficial** de cada ferramenta para iniciar projetos/pacotes (`bun init`, `bun create vite`, `shadcn init/add`, `pio project init`...). Nunca criar a estrutura inteira à mão.
