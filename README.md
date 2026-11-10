# HIJACK

Demonstração acadêmica de segurança em sistemas ciberfísicos: carrinho(s) ESP32, pilotos (celular) e hackers (notebook) numa LAN isolada. A primeira construção é de **um carrinho** (uma equipe pilota e defende, a outra ataca); dois carrinhos são opcionais.

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
2. **Montagem:** faça as ligações de `esp32/README.md` (servo SG90 no GPIO 13; motor esquerdo ENA 25, IN1 26, IN2 27; motor direito ENB 21, IN3 18, IN4 19; jumpers ENA/ENB retirados; ESP32 e servo no regulador HW-411 de 5 V; GND comum). A especificação completa do carrinho está em `esp32/docs/especificacao/`.
3. **Firmware:** em `esp32/`, grave com as saídas desligadas (`pio run -e esp32dev -t upload`). Pelo celular, entre na rede `HIJACK-…` que o carrinho cria e configure no portal a Wi-Fi do laboratório (2,4 GHz), o IP do notebook, `car-a`/`car-b` e o segredo do servidor (`esp32/README.md`, seção 5).
4. **Conferir:** monitor serial mostra `[ws] autenticado`; no `/admin`, troque o carrinho para **Hardware** e veja **online**.
5. **Calibrar e ligar as saídas:** pino e pulsos do servo, sentido dos motores, velocidade (`esp32/README.md`, seção 8), com o carrinho **suspenso**.

## Convenções

Leia `AGENTS.md` (regras para quem continuar o projeto, inclusive agentes de IA). Estado e pendências: `web/docs/progress.md` e `web/docs/acceptance.md`.
