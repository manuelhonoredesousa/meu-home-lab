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

## Configurar e testar o SSH

### Confirmar se o SSH está funcionando

No Ubuntu Server, execute:

```bash
sudo systemctl status ssh
```

O serviço deve aparecer como `active (running)`.

Se o SSH não estiver instalado, execute:

```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh
```

### Testar a ligação a partir do Windows

No PowerShell do Windows, substitua o usuário e o endereço IP pelos dados do seu servidor:

```powershell
ssh sousa@192.168.8.10
```

Na primeira ligação, será exibida uma mensagem semelhante a esta:

```text
The authenticity of host '192.168.8.10' can't be established.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Digite `yes` e, depois, informe a senha do usuário do Ubuntu Server. Se a ligação for bem-sucedida, verá um prompt semelhante a:

```text
sousa@pcServerName:~$
```

### Sair do servidor

Para encerrar a sessão SSH:

```bash
exit
```

## Criar uma chave SSH no Windows

No PowerShell do Windows, execute:

```powershell
ssh-keygen -t ed25519 -C "sousa-pcServerName"
```

Quando aparecer `Enter file in which to save the key`, pressione `Enter` para aceitar o local padrão.

Quando aparecer `Enter passphrase`, escolha uma das opções:

- Pressionar `Enter` para não usar uma senha na chave.
- Definir uma passphrase para proteger a chave privada.

### Arquivos criados

No Windows, as chaves ficarão normalmente em:

```text
C:\Users\TEU_USUARIO\.ssh\
```

Serão criados:

- `id_ed25519`: chave privada. Nunca compartilhe este arquivo.
- `id_ed25519.pub`: chave pública.

## Copiar a chave pública para o servidor

No PowerShell do Windows, mostre a chave pública:

```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub
```

Copie a linha inteira, que será semelhante a:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI...
```

Conecte-se ao servidor usando a senha:

```powershell
ssh sousa@192.168.8.10
```

No servidor, crie a pasta de chaves e ajuste suas permissões:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Abra o arquivo de chaves autorizadas:

```bash
nano ~/.ssh/authorized_keys
```

Cole a chave pública inteira e salve o arquivo:

1. Pressione `Ctrl+O`.
2. Pressione `Enter`.
3. Pressione `Ctrl+X`.

Ajuste as permissões do arquivo:

```bash
chmod 600 ~/.ssh/authorized_keys
```

## Testar a chave SSH

Primeiro, saia do servidor:

```bash
exit
```

No PowerShell do Windows, conecte-se novamente:

```powershell
ssh sousa@192.168.8.10
```

Se tudo estiver correto, entrará no servidor sem precisar informar a senha da conta. Se criou uma passphrase, poderá ser necessário informá-la. A passphrase protege a chave e é diferente da senha do usuário do servidor.

## Criar um atalho SSH (opcional)

No Windows, crie ou edite o arquivo abaixo:

```text
C:\Users\TEU_USUARIO\.ssh\config
```

Adicione o seguinte conteúdo:

```ssh-config
Host pcServerName
    HostName 192.168.8.10
    User sousa
    IdentityFile ~/.ssh/id_ed25519
```

Depois, conecte-se usando apenas o nome do atalho:

```powershell
ssh pcServerName
```

## Administrar o servidor com o Tabby

Como alternativa ao PowerShell, pode usar o [Tabby](https://tabby.sh/):

1. Abra o Tabby.
2. Clique em **New Profile**.
3. Escolha **SSH Connection**.
4. Preencha os dados:

    - **Name:** `pcServerName`
    - **Host:** `192.168.8.10`
    - **User:** `sousa`
    - **Authentication:** selecione a chave privada `id_ed25519`.

    A chave normalmente fica em um destes locais:

    ```text
    ~/.ssh/id_ed25519
    C:\Users\TEU_USUARIO\.ssh\id_ed25519
    ```

5. Salve o perfil.

Depois, para administrar o servidor, basta abrir o perfil `pcServerName`.

Outra alternativa para manipular arquivos com uma interface gráfica é o [Snowflake](https://github.com/subhra74/snowflake/).

## Acessar o servidor pelo VS Code

No computador Windows:

1. Abra o VS Code.
2. Instale a extensão **Remote - SSH**.
3. Pressione `Ctrl+Shift+P`.
4. Procure por `Remote-SSH: Connect to Host`.
5. Escolha `pcServerName`.

## Criar a pasta principal dos projetos

Depois de conectar ao servidor pelo VS Code, abra o terminal integrado e execute:

```bash
mkdir -p ~/projetos
```

## Configurar o Git no servidor

### Verificar a instalação

```bash
git --version
```

Se o Git não estiver instalado:

```bash
sudo apt install git -y
```

### Configurar o usuário do Git

Substitua os valores pelos seus dados:

```bash
git config --global user.name "Teu Nome"
git config --global user.email "teu-email@example.com"
```
