# Meu Home Lab

Documentação do meu laboratório doméstico, organizada por área para facilitar a instalação, a configuração, a manutenção e a monitorização dos serviços.

## Checklist rápido

Antes de começar, vale lembrar:

- manter o Ubuntu atualizado
- configurar rede estática ou reserva DHCP
- activar SSH e preferencialmente usar chave pública
- criar backups simples e regulares
- documentar endereços IP, portas e credenciais
- monitorizar CPU, memória, disco e rede

## Índice

- [Instalação e configuração do Ubuntu Server](pages/setup.md)
- [Monitorização do sistema](pages/monitoracao.md)
- [Terminal e comandos úteis](pages/terminal.md)
- [Odysseus AI](pages/odysseus.md)

## Guias

### Ubuntu Server

Consulte o guia de [instalação e configuração do Ubuntu Server](pages/setup.md) para configurar a rede, ativar o SSH, criar uma chave de acesso e preparar o servidor base.

### Monitorização

Veja o guia de [monitorização do sistema](pages/monitoracao.md) para acompanhar o uso de CPU, memória, processos, armazenamento e rede.

### Terminal e comandos úteis

Use a página de [terminal e comandos úteis](pages/terminal.md) como referência rápida para operações do sistema, rede, permissões, serviços e diagnóstico.

### Odysseus

Siga o guia de [instalação do Odysseus](pages/odysseus.md) para preparar um ambiente local com IA autogerida.

## Tópicos relevantes para o home lab

- configuração de rede e nomes de hosts
- acesso remoto via SSH
- gestão de serviços com `systemctl`
- uso de Docker e containers
- monitorização e alertas básicos
- armazenamento e backups
- segurança e boas práticas de autenticação

## Boas práticas

- usar `sudo` apenas quando necessário
- manter os serviços em portas conhecidas e documentadas
- configurar firewall e regras mínimas
- manter registos de versões e alterações
- testar sempre um serviço após reiniciar ou alterar a configuração
