# Monitorização do sistema

Ferramentas úteis para acompanhar o desempenho e o estado do Ubuntu Server.

## Índice

- [Monitorização geral](#monitorização-geral)
- [Disco e armazenamento](#disco-e-armazenamento)
- [Rede](#rede)
- [Sumários rápidos](#sumários-rápidos)
- [Para Gerenciar o Servidor (Ubuntu)](#para-gerenciar-o-servidor-ubuntu)
- [Para Gerenciar o Docker](#para-gerenciar-o-docker)
- [Para Monitoramento Avançado e Gráficos](#para-monitoramento-avançado-e-gráficos)

## Monitorização geral

### `top`

O utilitário nativo mais básico. Mostra os processos ativos e o uso dos recursos do sistema. Já vem instalado no Ubuntu Server.

### `htop`

Uma versão mais visual e interativa do `top`. Permite usar o rato e apresenta gráficos de barras coloridos.

Para instalar:

```bash
sudo apt install htop
```

### `btop`

Uma interface moderna e organizada para o terminal. Mostra gráficos detalhados de CPU, memória, discos e rede.

Para instalar:

```bash
sudo apt install btop
```

## Disco e armazenamento

### `df -h`

Mostra o espaço total, usado e disponível em todas as partições do sistema, usando unidades legíveis como GB e MB.

```bash
df -h
```

### `iotop`

Identifica quais processos estão a ler ou escrever mais dados no disco. É útil para investigar lentidão causada por operações de armazenamento.

Para instalar:

```bash
sudo apt install iotop
```

Para executar:

```bash
sudo iotop
```

## Rede

### `nload`

Exibe gráficos simples, em tempo real, do tráfego de entrada e saída da rede.

Para instalar:

```bash
sudo apt install nload
```

### `iftop`

Mostra detalhadamente quais ligações de rede e endereços IP estão a consumir a largura de banda.

Para instalar:

```bash
sudo apt install iftop
```

Para executar:

```bash
sudo iftop
```

## Sumários rápidos

### `glances`

Uma ferramenta avançada que reúne CPU, carga do sistema, memória, rede e disco num único ecrã condensado.

Para instalar:

```bash
sudo apt install glances
```

Para executar:

```bash
glances
```

## Para Gerenciar o Servidor (Ubuntu)

### Cockpit

**O que faz:** painel web para gerenciar o Ubuntu Server.

**Funções:** monitora CPU, memória e discos, gerencia usuários e atualizações e oferece um terminal web integrado.

**Instalação:**

```bash
sudo apt update
sudo apt install cockpit -y
sudo systemctl enable --now cockpit.socket
```

Acesse `https://IP_DO_SERVIDOR:9090` no navegador.

## Para Gerenciar o Docker

### Portainer CE

**O que faz:** interface web para administrar o Docker.

**Funções:** cria, inicia, pausa e remove containers, gerencia volumes e redes e mostra logs em tempo real.

**Instalação:**

```bash
docker volume create portainer_data
docker run -d \
	-p 8000:8000 \
	-p 9443:9443 \
	--name portainer \
	--restart=always \
	-v /var/run/docker.sock:/var/run/docker.sock \
	-v portainer_data:/data \
	portainer/portainer-ce:lts
```

Acesse `https://IP_DO_SERVIDOR:9443` no navegador.

## Para Monitoramento Avançado e Gráficos

### Netdata

**O que faz:** monitoramento visual em tempo real do servidor e dos containers Docker.

**Funções:** apresenta gráficos interativos de CPU, memória, discos, rede e outros recursos do sistema.

**Instalação:**

```bash
wget -O /tmp/netdata-kickstart.sh https://get.netdata.cloud/kickstart.sh
sudo sh /tmp/netdata-kickstart.sh
```

Acesse `http://IP_DO_SERVIDOR:19999` no navegador.
