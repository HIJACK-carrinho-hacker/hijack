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

## Subir do zero numa máquina nova (dia da apresentação)

Cenário: o celular do grupo faz o **hotspot** (roteador). O notebook que roda o servidor, o carrinho e o celular do piloto entram nessa mesma rede. O piloto abre no navegador o IP do notebook.

### 1. Ligar o hotspot

No celular, ative o roteador Wi-Fi em **2,4 GHz** (o ESP32 não enxerga 5 GHz). Android: Roteador Wi-Fi → Banda → 2,4 GHz. iPhone: Acesso Pessoal → ativar **Maximizar Compatibilidade**. Conecte o notebook nessa rede.

### 2. Instalar Git e Bun

Linux:

```bash
sudo pacman -S git            # Arch; no Ubuntu/Debian: sudo apt install git curl
curl -fsSL https://bun.sh/install | bash
exec $SHELL                   # recarrega o PATH para achar o bun
bun --version
```

Windows (PowerShell):

```powershell
winget install Git.Git
powershell -c "irm bun.sh/install.ps1 | iex"
# feche e abra o PowerShell de novo
bun --version
```

### 3. Baixar e configurar

Igual no Linux e no Windows (no Windows, pelo PowerShell):

```bash
git clone https://github.com/HIJACK-carrinho-hacker/hijack.git
cd hijack
git clone https://github.com/HIJACK-carrinho-hacker/hijack-web.git web
cd web
bun install
```

O `apps/server/.env` já vem no clone, com a senha do `/admin` (`ADMIN_SECRET`) e o mesmo `DEVICE_SECRET_CAR_A` gravado no carrinho. Não precisa criar nem editar nada. Se trocar o `DEVICE_SECRET_CAR_A`, grave o novo no carrinho pelo portal (passo 7).

### 4. Rodar

```bash
bun run demo
```

O terminal mostra algo como `[hijack] na LAN: http://10.143.22.57:3000`. Deixe esse terminal aberto durante toda a apresentação.

### 5. Descobrir o IP do notebook

O servidor já imprime o IP (passo 4). O jeito mais fácil é rodar, em outro terminal dentro de `web/`:

```bash
bun run ip
```

Ele mostra o IP da placa Wi-Fi primeiro, já com os endereços do piloto, do `/admin` e o que digitar no portal do carrinho (funciona no Linux, no Windows e no macOS). Se aparecer mais de uma placa, use a conectada ao hotspot. Para conferir sem o Bun:

| Sistema | Comando | Onde olhar |
|---|---|---|
| Linux | `ip -4 -br addr` | linha da placa Wi-Fi (`wlan0`, `wlp…`), número antes da `/` |
| Windows (PowerShell) | `Get-NetIPAddress -AddressFamily IPv4 -InterfaceAlias Wi-Fi` | campo `IPAddress` |
| Windows (qualquer terminal) | `ipconfig` | bloco **Adaptador de Rede sem Fio Wi-Fi** → **Endereço IPv4** |

Ignore `127.0.0.1` e endereços de Docker ou VPN. O IP costuma começar com `192.168.`, `10.` ou, no iPhone, `172.20.10.`.

No celular do piloto (conectado ao hotspot, ou o próprio celular do hotspot), abra `http://<IP>:3000`. É `http`, não `https`. O operador abre `http://<IP>:3000/admin`.

### 6. Liberar o firewall do notebook

- Linux com UFW: `sudo ufw allow 3000/tcp`
- Linux com firewalld: `sudo firewall-cmd --add-port=3000/tcp`
- Windows: na primeira execução do `bun`, aceite o aviso do firewall marcando **Redes privadas e públicas**. Se o celular não abrir a página, rode no PowerShell como administrador: `New-NetFirewallRule -DisplayName HIJACK -Direction Inbound -Protocol TCP -LocalPort 3000 -Action Allow`

### 7. Apontar o carrinho para o notebook

O carrinho guarda a rede Wi-Fi, o IP do notebook e o segredo. Se a rede, o notebook ou o IP mudaram, reconfigure:

1. Ligue o carrinho e segure o botão **BOOT** do ESP32 por 3 s.
2. No celular, entre na rede `HIJACK-car-a` (senha `hijack-config`, se não foi trocada). Se a página não abrir sozinha, acesse `http://192.168.4.1`.
3. Em **Configure WiFi**, escolha o hotspot, digite a senha dele e preencha: IP do notebook (passo 5), porta `3000`, carrinho `car-a` e o mesmo `DEVICE_SECRET_CAR_A` do `.env`. Toque em **Save**.
4. No `/admin`, troque o Carrinho A para **Hardware** (fora de "Em demonstração"). O cartão deve mostrar **online**.

O IP do notebook no hotspot pode mudar quando ele se desconecta e volta. Antes de começar, rode o comando do passo 5 de novo. Se o IP mudou, refaça este passo.

Mais detalhes sobre o portal em `esp32/README.md`, seção 5. Para regravar o firmware também é preciso clonar o `hijack-esp32` em `esp32/` e instalar o PlatformIO (`esp32/README.md`).

## Rodar só a simulação (sem carrinhos)

```bash
cd web
bun install
bun run demo
```

Abra o endereço impresso no terminal (`http://<IP-do-notebook>:3000`). Detalhes e páginas em `web/README.md`.

No painel do hacker (`/hacker`), os 12 controles de ataque e as 4 defesas têm um ícone de informação. Passe o mouse, clique ou toque para ver **Na vida real** (o cenário de segurança) e **No HIJACK** (o comportamento e os limites da demonstração). As explicações também abrem por teclado e não ensinam a executar ataques. O roteiro para apresentar esses exemplos está em `web/docs/demo-script.md`.

## Rodar com os carrinhos reais

1. **Servidor:** siga o passo acima no notebook. Anote o IP e os valores de `DEVICE_SECRET_CAR_A` / `_B` em `web/apps/server/.env`.
2. **Montagem:** faça as ligações de `esp32/README.md` (servo SG90 no GPIO 13; motor esquerdo ENA 25, IN1 26, IN2 27; motor direito ENB 21, IN3 18, IN4 19; jumpers ENA/ENB retirados; ESP32 e servo no regulador HW-411 de 5 V; GND comum). A especificação completa do carrinho está em `esp32/docs/especificacao/`.
3. **Firmware:** em `esp32/`, grave com as saídas desligadas (`pio run -e esp32dev -t upload`). Pelo celular, entre na rede `HIJACK-…` que o carrinho cria e configure no portal a Wi-Fi do laboratório (2,4 GHz), o IP do notebook, `car-a`/`car-b` e o segredo do servidor (`esp32/README.md`, seção 5).
4. **Conferir:** monitor serial mostra `[ws] autenticado`; no `/admin`, troque o carrinho para **Hardware** e veja **online**.
5. **Calibrar e ligar as saídas:** pino e pulsos do servo, sentido dos motores, velocidade (`esp32/README.md`, seção 8), com o carrinho **suspenso**.
6. **Rede e bancada:** para a demonstração use roteador dedicado de 2,4 GHz e o ESP32 perto dele. A ordem prática dos testes de hardware, conforme o material chega, está em `esp32/docs/proximos-passos-hardware.md`.

## Relatório final

O relatório da disciplina (UFRN, Fundamentos de Circuitos e Sistemas Controlados) está em `relatorio/`: fonte LaTeX no modelo ABNT `abntex2` (`relatorio.tex`), referências (`referencias.bib`), imagens do modelo 3D (`figuras/`) e o PDF pronto (`relatorio.pdf`).

Para compilar é preciso o TeX Live com o `abntex2` (no Arch: `sudo pacman -S texlive-publishers`). Dentro de `relatorio/`:

```bash
pdflatex relatorio && bibtex relatorio && pdflatex relatorio && pdflatex relatorio
```

## Apresentação

Os slides da apresentação (Beamer, tema metropolis, 16:9) estão em `apresentacao/`: `apresentacao.tex`, `imagens/` e `apresentacao.pdf`. Compilar dentro da pasta com `pdflatex apresentacao` duas vezes. As fotos de componentes, do porto, do supermercado e os logos vêm do Wikimedia Commons, com autor e licença em cada slide; as imagens do carrinho são renderizações do modelo 3D.

## Convenções

Leia `AGENTS.md` (regras para quem continuar o projeto, inclusive agentes de IA). Estado e pendências: `web/docs/progress.md` e `web/docs/acceptance.md`.
