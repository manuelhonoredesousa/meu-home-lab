

# Acesso à rede local pelo Tailscale

Este guia configura o `beta`, um computador com Ubuntu Server, como subnet
router do Tailscale. Assim, outros dispositivos ligados ao Tailscale podem
aceder à rede local através dele.

## Topologia da rede

No `beta`, confirme as rotas existentes:

```bash
ip route
```

Um resultado semelhante a este indica que a rede local está configurada:

```text
default via 192.168.0.1 dev enp0s31f6 proto static
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1
172.18.0.0/16 dev br-4c86f322d263 proto kernel scope link src 172.18.0.1
172.19.0.0/16 dev br-0da38351cf1d proto kernel scope link src 172.19.0.1
192.168.0.0/24 dev enp0s31f6 proto kernel scope link src 192.168.0.2
```

Os endereços relevantes são:

| Equipamento ou rede | Endereço |
| --- | --- |
| `beta` | `192.168.0.2` |
| Roteador | `192.168.0.1` |
| Rede local | `192.168.0.0/24` |

## 1. Anunciar a rede pelo Tailscale

Execute no terminal do `beta`:

```bash
sudo tailscale set --advertise-routes=192.168.0.0/24
```

Este comando informa ao Tailscale que o `beta` consegue alcançar a rede
`192.168.0.0/24` e pode encaminhar para ela o tráfego de outros dispositivos
Tailscale. Ele não altera o IP do `beta`, não muda o roteador e não transforma
o computador num exit node.

## 2. Confirmar a ligação ao Tailscale

Ainda no `beta`, verifique o estado da ligação:

```bash
tailscale status
```

O `beta` deverá aparecer com um endereço Tailscale, por exemplo:

```text
100.96.41.36    beta                 sousa64manuel@  linux -
100.95.209.118  manuels-macbook-pro  sousa64manuel@  macOS  active
```

Neste exemplo, o IP Tailscale do `beta` é `100.96.41.36`.

## 3. Confirmar a rota anunciada

O comando `tailscale status` confirma a ligação, mas não mostra
necessariamente as rotas anunciadas. Para verificar essa configuração:

```bash
tailscale debug prefs
```

Procure esta entrada no resultado:

```json
"AdvertiseRoutes": [
    "192.168.0.0/24"
]
```

Isso confirma que o `beta` está configurado para anunciar a rede local.

## 4. Aprovar a rota no painel

1. Abra o [painel de máquinas do Tailscale](https://console.tailscale.com/admin/machines).
2. Procure o computador `beta`.
3. Abra o menu `...` e selecione `Edit route settings`.
4. Ative ou aprove a rota `192.168.0.0/24` em `Subnet routes`.

Sem esta aprovação, a rota pode estar anunciada pelo `beta`, mas não será
utilizada pelos outros dispositivos Tailscale.

## 5. Testar o acesso à rede local

No HP, ligado ao Tailscale, abra o navegador e aceda ao painel do roteador:

```text
http://192.168.0.1
```

Se a página do roteador abrir, o acesso à rede local através do `beta` está a
funcionar.