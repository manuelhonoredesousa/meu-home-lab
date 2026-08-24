# Meu Home Lab

## Instalação do Ubuntu Server

### Download

Baixe o Ubuntu Server pelo site oficial:

[Download do Ubuntu Server](https://ubuntu.com/download/server)

### Processo de instalação

Quando o instalador carregar, siga os passos abaixo no menu de texto:

1. **Idioma:** selecione o idioma do instalador. Inglês ou Português são opções recomendadas e, em seguida, pressione `Enter`.

2. **Layout do teclado:** escolha `Portuguese (Brazil)` ou `Portuguese`, caso esteja em Portugal. Teste as teclas e selecione `Done`.

3. **Tipo de instalação:** escolha entre `Ubuntu Server` e `Ubuntu Server (minimized)`. Selecione a versão padrão.

4. **Rede (Network):** o sistema tentará obter um endereço IP automaticamente via DHCP. Se estiver tudo correto, selecione `Done` ou escolhe Estatico-> Manual, e use:
    - Subnet: 192.168.0.0/24
    - Address: 192.168.0.2
    - Gatway: 192.168.0.1
    - Name Server: 8.8.8.8, 1.1.1.1
    - Search domain: deixa vazio, se não tiver
    -- Confirma e espera aparecer a confirmação

5. **Proxy e Mirror:**
    - Se não usa um proxy, deixe o campo em branco.
    - Na tela `Mirror`, mantenha o endereço padrão sugerido para baixar as atualizações.

6. **Armazenamento (Storage):**
    - Escolha `Use an entire disk` para usar o disco inteiro.
    - Mantenha marcada a opção de criar um grupo LVM.
    - Avance até o resumo das partições e selecione `Done`.
    - No aviso de confirmação, escolha `Continue`. Essa ação apagará os dados do disco selecionado.

    Para usar todo o espaço disponível:

    1. Acesse `USED DEVICES`.
    2. Selecione o dispositivo e, depois, `ubuntu-lv`.
    3. Escolha `Edit`.
    4. Defina o tamanho máximo disponível.

7. **Perfil do usuário:** preencha os dados básicos do servidor:
    - `Your name`: Manuel de Sousa.
    - `Your server's name`: nome da máquina na rede, por exemplo, `beta`.
    - `Pick a username`: nome de usuário para login, por exemplo, `sousa`.
    - `Choose a password` / `Confirm`: defina e confirme uma senha forte.

8. **Ubuntu Pro:** selecione `Skip for now` para deixar a ativação do Ubuntu Pro para depois.

9. **Acesso remoto (SSH):** marque `Install OpenSSH server` pressionando a barra de espaço. Isso permitirá administrar o servidor remotamente. Em seguida, selecione `Done`.

10. **Snaps de servidor:** será exibida uma lista de programas populares, como Docker e Nextcloud. Se não precisar instalar nenhum deles agora, selecione `Done` para iniciar a instalação.

---

## Configurar e testar a rede

### Ver o IP atual do servidor

Use um dos comandos abaixo:

```bash
ip a
```

Ou, de forma mais simples:

```bash
hostname -I
```

O resultado deve mostrar um endereço parecido com `192.168.8.123`.

### Testar a ligação com a internet

```bash
ping www.google.com
```

Para interromper o teste, pressione `Ctrl+C`.

### Atualizar o Ubuntu

Execute os comandos abaixo:

```bash
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
```

### Descobrir o nome da interface de rede

Execute:

```bash
ip a
```

Procure o nome da interface de rede. Exemplos:

- `enp0s31f6`
- `eno1`
- `eth0`
