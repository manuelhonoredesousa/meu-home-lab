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

## Instalar Docker e Docker Compose

### Atualizar o sistema

```bash
sudo apt update
sudo apt upgrade -y
```

### Instalar os pacotes necessários

```bash
sudo apt install ca-certificates curl gnupg -y
```

### Adicionar o repositório oficial do Docker

Crie o diretório para as chaves:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

Adicione a chave oficial do Docker:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

Ajuste a permissão da chave:

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

Adicione o repositório:

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Atualize a lista de pacotes:

```bash
sudo apt update
```

### Instalar o Docker

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

Verifique as versões instaladas:

```bash
docker --version
docker compose version
```

Os comandos devem mostrar as versões do Docker e do Docker Compose.

### Testar o Docker

```bash
sudo docker run hello-world
```

Se a mensagem de boas-vindas for exibida, o Docker está funcionando.

### Usar o Docker sem `sudo`

Por padrão, seria necessário executar comandos como `sudo docker ps`. Para permitir o uso do Docker diretamente pelo usuário atual:

```bash
sudo usermod -aG docker $USER
```

Encerre a sessão SSH:

```bash
exit
```

Conecte-se novamente:

```powershell
ssh pcServerName
```

Teste o acesso sem `sudo`:

```bash
docker ps
```

### Criar a estrutura de diretórios

Crie a pasta principal e suas subpastas:

```bash
sudo mkdir -p /opt/pcServerName/{stacks,data,backups,scripts}
```

A estrutura criada será:

```text
/opt/pcServerName/
├── backups
├── data
├── scripts
└── stacks
```

### Criar a rede Docker

Crie uma rede para os serviços:

```bash
docker network create pcServerName
```

Verifique se a rede foi criada:

```bash
docker network ls
docker network inspect pcServerName
```

## Instalar o Portainer

O Portainer é uma interface web para administrar o Docker:

```text
PC Windows (navegador)
                    |
                    v
https://192.168.8.10:9443
                    |
                    v
            Portainer
                    |
                    v
                Docker
```

### Criar o volume do Portainer

No Ubuntu Server, execute:

```bash
docker volume create portainer_data
docker volume ls
```

Deve aparecer o volume `portainer_data`, que guarda os dados e as configurações do Portainer.

### Criar o container

```bash
docker run -d \
    -p 8000:8000 \
    -p 9443:9443 \
    --name portainer \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer-ce:latest
```

Verifique o container:

```bash
docker ps
```

Procure por `portainer/portainer-ce` com um estado semelhante a `Up`.

### Abrir o Portainer

No navegador do Windows, acesse:

[https://192.168.8.10:9443](https://192.168.8.10:9443)

Use `https`, e não `http`. Como o Portainer usa inicialmente um certificado próprio, o navegador pode mostrar um aviso de certificado. Isso é esperado no primeiro acesso dentro da rede local.

### Criar a conta administrativa

Na primeira página, crie um usuário e uma senha forte e exclusiva. Não reutilize a senha do Ubuntu.

Depois de entrar no painel, acesse **Environments > local**. Deverá conseguir visualizar:

- Containers
- Images
- Networks
- Volumes
- Stacks

O Portainer é apenas uma interface para administrar o Docker. Os comandos abaixo continuam disponíveis:

```bash
docker ps
docker logs <container>
docker exec -it <container> <comando>
docker compose up -d
```

> **Atenção:** não exponha a porta `9443` diretamente para a internet. Neste momento, use o Portainer somente pela rede local.

## PostgreSQL em Docker

A estrutura será:

```text
/opt/pcServerName/
└── stacks/
        └── postgres/
                ├── compose.yml
                └── .env
```

Os dados ficarão em um volume Docker, separados do container.

### Criar a pasta do PostgreSQL

No servidor, execute:

```bash
sudo mkdir -p /opt/pcServerName/stacks/postgres
cd /opt/pcServerName/stacks/postgres
pwd
```

O comando `pwd` deve mostrar:

```text
/opt/pcServerName/stacks/postgres
```

### Criar o arquivo `.env`

As credenciais ficarão fora do `compose.yml`:

```bash
sudo nano .env
```

Adicione o conteúdo abaixo e substitua a senha:

```dotenv
POSTGRES_USER=sousa
POSTGRES_PASSWORD=COLOQUE_UMA_SENHA_FORTE_AQUI
POSTGRES_DB=pcServerName
```

No `nano`, salve com `Ctrl+O`, pressione `Enter` e saia com `Ctrl+X`. Depois proteja o arquivo:

```bash
sudo chmod 600 .env
```

### Criar o Docker Compose

```bash
sudo nano compose.yml
```

Adicione:

```yaml
services:
    postgres:
        image: postgres:18
        container_name: postgres
        restart: unless-stopped

        environment:
            POSTGRES_USER: ${POSTGRES_USER}
            POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
            POSTGRES_DB: ${POSTGRES_DB}

        volumes:
            - postgres_data:/var/lib/postgresql

        ports:
            - "127.0.0.1:5432:5432"

        networks:
            - pcServerName

volumes:
    postgres_data:

networks:
    pcServerName:
        external: true
```

> **Nota:** `127.0.0.1:5432:5432` limita o acesso direto ao próprio servidor. Outros computadores da rede não poderão acessar o PostgreSQL por essa porta. Os containers conectados à rede Docker `pcServerName` continuarão podendo comunicar-se com ele.

### Iniciar o PostgreSQL

Na pasta `/opt/pcServerName/stacks/postgres`, execute:

```bash
docker compose up -d
docker ps
```

### Ver os logs

```bash
docker logs postgres
```

Procure uma mensagem semelhante a:

```text
database system is ready to accept connections
```

Para acompanhar os logs em tempo real:

```bash
docker logs -f postgres
```

Pressione `Ctrl+C` para sair.

### Testar o PostgreSQL

Entre no PostgreSQL dentro do container:

```bash
docker exec -it postgres psql -U sousa -d pcServerName
```

Se estiver funcionando, o prompt será semelhante a:

```text
pcServerName=#
```

Dentro do PostgreSQL, execute:

```sql
SELECT version();
\l
\dt
\q
```

O comando `\q` encerra o cliente do PostgreSQL.

### Verificar o volume

```bash
docker volume ls
```

Deverá aparecer um volume semelhante a `postgres_postgres_data`, dependendo do nome do projeto Compose. Esse volume mantém os dados persistentes.

Ao executar `docker compose down`, o container é removido, mas o volume continua existindo. Ao executar `docker compose up -d` novamente, o PostgreSQL volta a utilizar os mesmos dados.

> **Atenção:** não use `docker compose down -v` sem saber exatamente o que está fazendo. A opção `-v` remove os volumes associados e pode apagar os dados persistentes.

## Instalar o pgAdmin em Docker

O pgAdmin será executado em um container e conectado ao PostgreSQL pela rede Docker `pcServerName`.

### Criar a pasta do pgAdmin

No Ubuntu Server, execute:

```bash
sudo mkdir -p /opt/pcServerName/stacks/pgadmin
cd /opt/pcServerName/stacks/pgadmin
```

### Criar o arquivo `.env`

```bash
sudo nano .env
```

Adicione o conteúdo abaixo e substitua a senha:

```dotenv
PGADMIN_DEFAULT_EMAIL=sousa@pcServerName.local
PGADMIN_DEFAULT_PASSWORD=COLOQUE_UMA_SENHA_FORTE
```

No `nano`, salve com `Ctrl+O`, pressione `Enter` e saia com `Ctrl+X`. Depois proteja o arquivo:

```bash
sudo chmod 600 .env
```

### Criar o Docker Compose

```bash
sudo nano compose.yml
```

Adicione:

```yaml
services:
    pgadmin:
        image: dpage/pgadmin4:latest
        container_name: pgadmin
        restart: unless-stopped

        environment:
            PGADMIN_DEFAULT_EMAIL: ${PGADMIN_DEFAULT_EMAIL}
            PGADMIN_DEFAULT_PASSWORD: ${PGADMIN_DEFAULT_PASSWORD}

        ports:
            - "5050:80"

        volumes:
            - pgadmin_data:/var/lib/pgadmin

        networks:
            - pcServerName

volumes:
    pgadmin_data:

networks:
    pcServerName:
        external: true
```

### Iniciar o pgAdmin

Na pasta `/opt/pcServerName/stacks/pgadmin`, execute:

```bash
docker compose up -d
docker ps
```

### Abrir o pgAdmin

No navegador do Windows, acesse:

[http://192.168.8.10:5050](http://192.168.8.10:5050)

Entre com:

- **Email:** `sousa@pcServerName.local`
- **Password:** a senha definida no arquivo `.env`

### Conectar o pgAdmin ao PostgreSQL

Dentro do pgAdmin, acesse **Servers > Register > Server**.

Na aba **General**:

- **Name:** `pcServerName PostgreSQL`

Na aba **Connection**, use:

- **Host name/address:** `postgres`
- **Port:** `5432`
- **Username:** `sousa`
- **Password:** a senha definida em `POSTGRES_PASSWORD`

> **Importante:** não use `localhost` nem `192.168.8.10` como host. Como o pgAdmin e o PostgreSQL estão na mesma rede Docker, o nome do container `postgres` é o endereço correto.

Salve a conexão. O PostgreSQL deverá aparecer na lista de servidores do pgAdmin.

## Configurar backups automáticos do PostgreSQL

Os backups serão criados com `pg_dumpall`, compactados com `gzip` e mantidos por 14 dias.

### Criar as pastas dos backups e scripts

```bash
sudo mkdir -p /opt/pcServerName/backups/postgres
sudo mkdir -p /opt/pcServerName/scripts
sudo chown -R sousa:sousa /opt/pcServerName/backups /opt/pcServerName/scripts
```

O último comando permite que o usuário `sousa`, que executará o cron, grave os backups e os logs.

### Criar o script de backup

```bash
nano /opt/pcServerName/scripts/backup-postgres.sh
```

Adicione:

```bash
#!/bin/bash

set -e

BACKUP_DIR="/opt/pcServerName/backups/postgres"
DATE=$(date +"%Y-%m-%d_%H-%M-%S")
BACKUP_FILE="$BACKUP_DIR/postgres_$DATE.sql"

mkdir -p "$BACKUP_DIR"

docker exec postgres pg_dumpall -U sousa > "$BACKUP_FILE"
gzip "$BACKUP_FILE"

find "$BACKUP_DIR" -type f -name "*.sql.gz" -mtime +14 -delete

echo "Backup criado: $BACKUP_FILE.gz"
```

Torne o script executável:

```bash
chmod +x /opt/pcServerName/scripts/backup-postgres.sh
ls -l /opt/pcServerName/scripts/
```

O arquivo deverá ter permissão de execução, semelhante a `-rwxr-xr-x`.

### Executar o primeiro backup manualmente

Teste o script antes de automatizá-lo:

```bash
/opt/pcServerName/scripts/backup-postgres.sh
ls -lh /opt/pcServerName/backups/postgres/
```

Deverá encontrar um arquivo semelhante a:

```text
postgres_2026-08-25_00-15-00.sql.gz
```

### Verificar o conteúdo do backup

Substitua `NOME_DO_ARQUIVO` pelo nome real do arquivo:

```bash
zcat /opt/pcServerName/backups/postgres/NOME_DO_ARQUIVO.sql.gz | head
```

Deverá aparecer conteúdo SQL.

### Automatizar o backup com Cron

Abra o cron do usuário `sousa`:

```bash
crontab -e
```

Se for a primeira vez, escolha `nano`. Adicione ao final:

```cron
0 2 * * * /opt/pcServerName/scripts/backup-postgres.sh >> /opt/pcServerName/backups/postgres/backup.log 2>&1
```

Essa configuração executa o backup todos os dias às `02:00`.

Confirme a configuração:

```bash
crontab -l
```

### Verificar os logs

Depois da execução do backup, consulte o log:

```bash
cat /opt/pcServerName/backups/postgres/backup.log
```

## Home Lab até agora

```text
Dell OptiPlex 5050
|
└── Ubuntu Server
        |
        ├── SSH
        │   └── PC Windows
        ├── Docker
        ├── Portainer
        ├── PostgreSQL
        │   └── Volume persistente
        ├── pgAdmin
        │   └── Administração gráfica
        └── Backup
                ├── pg_dumpall
                ├── Compressão
                ├── Cron
                └── Retenção de 14 dias
```

## Acesso pela internet e firewall

Por enquanto, mantenha os serviços acessíveis apenas pela rede local. Se precisar configurar acesso pela internet, consulte a [documentação original sobre firewall](https://chatgpt.com/share/6a8cd4fb-6338-83ea-a42f-6cf9ca409eee) e configure também o roteador e as regras de segurança necessárias.

> **Atenção:** não exponha diretamente à internet as portas do PostgreSQL (`5432`), pgAdmin (`5050`) ou a porta de desenvolvimento do Next.js (`3000`). O Portainer (`9443`) também deve ser protegido antes de qualquer acesso externo.

## Preparar o ambiente de projetos

### Criar a pasta de projetos

No Ubuntu Server, execute:

```bash
mkdir -p ~/projetos
cd ~/projetos
pwd
```

### Instalar NVM, Node.js, pnpm e Bun

Instale o NVM (Node Version Manager):

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
source ~/.bashrc
```

Instale a versão LTS do Node.js:

```bash
nvm install --lts
nvm use --lts
node --version
npm --version
```

Ative o pnpm pelo Corepack:

```bash
corepack enable
corepack prepare pnpm@latest --activate
pnpm --version
```

Instale o Bun:

```bash
curl -fsSL https://bun.sh/install | bash
source ~/.bashrc
bun --version
```

### Criar uma aplicação Next.js

Na pasta `~/projetos`, execute uma das opções:

```bash
npx create-next-app@latest
```

Ou, usando pnpm ou Bun:

```bash
pnpm create next-app@latest
bunx create-next-app@latest
```

Entre na pasta criada e inicie o servidor de desenvolvimento:

```bash
cd nome-do-projeto
pnpm run dev
```

Por padrão, a aplicação estará disponível em `http://localhost:3000` no Ubuntu Server. Esse `localhost` representa o servidor, e não o computador Windows.

Para permitir o acesso pela rede local, inicie o Next.js escutando em todas as interfaces:

```bash
pnpm run dev -- -H 0.0.0.0
```

Depois, abra no navegador do Windows:

[http://192.168.8.10:3000](http://192.168.8.10:3000)

### Testar o Hot Reload

Abra o projeto pelo **VS Code > Remote SSH** e edite:

```text
src/app/page.tsx
```

Altere algum texto e salve o arquivo. O navegador deverá atualizar automaticamente.

```text
PC Windows
        |
        | VS Code Remote SSH
        v
Ubuntu Server
        |
        ├── Arquivos do projeto
        ├── Node.js
        └── Next.js
                        |
                        v
            Navegador Windows
```

## Configurar a conexão com o banco

Como o Next.js está sendo executado diretamente no Ubuntu Server e o PostgreSQL está exposto apenas em `127.0.0.1:5432`, o arquivo `.env` do projeto pode usar:

```dotenv
DATABASE_URL="postgresql://sousa:TUA_SENHA@127.0.0.1:5432/meu_primeiro_app?schema=public"
```

Substitua `TUA_SENHA` pela senha definida em `POSTGRES_PASSWORD`.

> **Nota:** usamos `127.0.0.1` porque o comando `pnpm run dev` está sendo executado diretamente no Ubuntu Server.

Quando o projeto também for executado em Docker, a URL deverá usar o nome do serviço PostgreSQL na rede Docker:

```dotenv
DATABASE_URL="postgresql://sousa:TUA_SENHA@postgres:5432/meu_primeiro_app?schema=public"
```

## Criar um modelo Prisma

Abra o arquivo:

```text
prisma/schema.prisma
```

Exemplo:

```prisma
generator client {
    provider = "prisma-client-js"
}

datasource db {
    provider = "postgresql"
    url      = env("DATABASE_URL")
}

model User {
    id        String   @id @default(cuid())
    name      String
    email     String   @unique
    createdAt DateTime @default(now())
}
```

