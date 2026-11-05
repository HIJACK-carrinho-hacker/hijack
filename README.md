# HIJACK

Demonstração acadêmica de segurança em sistemas ciberfísicos: dois carrinhos ESP32, pilotos (celular) e hackers (notebook) numa LAN isolada.

O projeto é dividido em três repositórios da organização [HIJACK-carrinho-hacker](https://github.com/HIJACK-carrinho-hacker):

| Repositório | O que é | Pasta local |
|---|---|---|
| [`hijack`](https://github.com/HIJACK-carrinho-hacker/hijack) | este: documentação geral e instruções para agentes | raiz |
| [`hijack-web`](https://github.com/HIJACK-carrinho-hacker/hijack-web) | aplicação web: servidor (Bun + Express + WebSocket) e telas (React) | `web/` |
| [`hijack-esp32`](https://github.com/HIJACK-carrinho-hacker/hijack-esp32) | firmware dos carrinhos (Arduino + PlatformIO, ESP32-WROOM-32 DevKit 38 pinos) | `esp32/` |

## Baixar tudo

Os dois projetos ficam **dentro** da pasta deste repositório (o `.gitignore` daqui ignora `web/` e `esp32/`):

```bash
git clone git@github.com:HIJACK-carrinho-hacker/hijack.git
cd hijack
git clone git@github.com:HIJACK-carrinho-hacker/hijack-web.git web
git clone git@github.com:HIJACK-carrinho-hacker/hijack-esp32.git esp32
```

Os nomes das pastas importam: a documentação de um projeto aponta para o outro como `../web` e `../esp32`.

## Rodar só a simulação (sem carrinhos)

```bash
cd web
bun install
cp apps/server/.env.example apps/server/.env   # troque os segredos
bun run demo
```

Abra o endereço impresso no terminal (`http://<IP-do-notebook>:3000`). Detalhes e páginas em `web/README.md`.

## Rodar com os carrinhos reais

1. **Servidor:** siga o passo acima no notebook. Anote o IP e os valores de `DEVICE_SECRET_CAR_A` / `_B` em `web/apps/server/.env`.
2. **Montagem:** faça as ligações da seção 3 de `esp32/README.md` (L298N nos GPIOs 14, 27, 26, 25, 33 e 32; retire os jumpers ENA/ENB; GND comum; servo com alimentação própria).
3. **Firmware:** em `esp32/`, crie `include/secrets.h` (Wi-Fi 2,4 GHz, IP do notebook, `car-a`/`car-b`, segredo do servidor) e grave com as saídas desligadas: `pio run -e esp32dev -t upload`.
4. **Conferir:** monitor serial mostra `[ws] autenticado`; no `/admin`, troque o carrinho para **Hardware** e veja **online**.
5. **Calibrar e ligar as saídas:** pino e pulsos do servo, sentido dos motores, velocidade (`esp32/README.md`, seção 8), com o carrinho **suspenso**.

## Convenções

Leia `AGENTS.md` (regras para quem continuar o projeto, inclusive agentes de IA). Estado e pendências: `web/docs/progress.md` e `web/docs/acceptance.md`.
