# File Browser

Este guia instala e configura o File Browser em Docker para gerir ficheiros do servidor através de uma interface web simples.

## 1. Objetivo

O File Browser permite:

- navegar pelos ficheiros e pastas do servidor
- fazer upload e download de ficheiros
- gerir permissões básicas
- aceder a uma pasta específica, como `/home/sousa/files`
- manter a gestão de arquivos num serviço leve e fácil de usar

## 2. Pré-requisitos

Antes de começar, confirme que o ambiente tem:

- Ubuntu Server atualizado
- Docker e Docker Compose instalados
- acesso ao diretório de armazenamento, por exemplo `/home/sousa/files`
- permissões para criar a pasta de configuração do serviço

## 3. Estrutura de pasta

Crie a estrutura necessária para guardar a base de dados e a configuração do File Browser:

```bash
mkdir -p ~/docker/filebrowser/config
cd ~/docker/filebrowser

touch config/filebrowser.db config/filebrowser.json
```

## 4. Criar o ficheiro de configuração

Crie o ficheiro `docker-compose.yml` com o conteúdo abaixo:

```bash
cd ~/docker/filebrowser
touch docker-compose.yml
nano docker-compose.yml
```

```yaml
services:
  filebrowser:
    image: filebrowser/filebrowser:latest
    container_name: filebrowser
    restart: unless-stopped
    ports:
      - "8080:80"
    volumes:
      - /home/sousa/files:/srv
      - ./config/filebrowser.db:/database.db
      - ./config/filebrowser.json:/config.json
    user: "1000:1000"
```

### Explicação rápida

- `8080:80`: expõe a interface web na porta `8080`
- `/home/sousa/files:/srv`: mapeia a pasta de ficheiros do servidor para o container
- `./config/filebrowser.db:/database.db`: guarda a base de dados do File Browser localmente
- `./config/filebrowser.json:/config.json`: guarda a configuração do serviço
- `user: "1000:1000"`: usa o utilizador local com as permissões correctas

## 5. Iniciar o serviço

```bash
docker compose up -d
```

Se tudo estiver correto, o container deve arrancar em segundo plano.

## 6. Aceder à interface web

Abra no navegador:

```text
http://IP_DO_SERVIDOR:8080
```

A primeira vez, é normalmente pedido:

- utilizador
- password
- criação da conta administrativa

## 7. Primeiros passos úteis

Depois de entrar:

- configure a pasta raiz para `/srv`
- escolha a pasta que pretende gerir
- use uma conta forte para o acesso web
- registe o endereço e a porta para facilitar o acesso futuro

## 8. Dicas de uso

- manter a pasta de arquivos em `/home/sousa/files` para organização simples
- guardar backups e documentos em estruturas claras por categoria
- limitar acesso a quem realmente precisa de usar o File Browser
- usar `docker compose logs` se o container não iniciar corretamente

## 9. Verificação rápida

```bash
docker ps
```

Se o container aparecer como `Up`, o serviço está a correr corretamente.

## 10. Comandos úteis

```bash
docker compose logs -f

docker compose down
docker compose up -d
```

Use estes comandos para:

- consultar logs
- reiniciar o serviço
- parar e voltar a iniciar em caso de alteração de configuração

## 11. Observações

Este serviço é simples, leve e útil para gerir arquivos em casa. Para um ambiente mais robusto, vale a pena combinar com autenticação forte, rede local controlada e backups regulares dos dados importantes.

