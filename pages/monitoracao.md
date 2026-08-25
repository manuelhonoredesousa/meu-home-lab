# Monitorização do sistema

Ferramentas úteis para acompanhar o desempenho e o estado do Ubuntu Server.

## Índice

- [Monitorização geral](#monitorização-geral)
- [Disco e armazenamento](#disco-e-armazenamento)
- [Rede](#rede)
- [Sumários rápidos](#sumários-rápidos)

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
