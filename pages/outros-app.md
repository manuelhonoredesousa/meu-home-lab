# Outras aplicações

Aplicações auto-hospedadas úteis para converter, comprimir e organizar ficheiros PDF.

## Índice

- [BentoPDF](#bentopdf)
- [BentoPDF Hyper Compress](#bentopdf-hyper-compress)

## BentoPDF

[Repositório do BentoPDF no GitHub](https://github.com/alam00000/bentopdf)

O BentoPDF disponibiliza ferramentas para trabalhar com ficheiros PDF através de uma interface web.

### Docker Compose

```yaml
services:
  bentopdf:
    # GitHub Container Registry (Recommended)
    # Self-Hosted build - ghcr.io/alam00000/bentopdf-simple:latest
    # Commercial build  - ghcr.io/alam00000/bentopdf:latest
    # Docker Hub (Alternative)
    # Self-Hosted build - bentopdfteam/bentopdf-simple:latest
    # Commercial build  - bentopdfteam/bentopdf:latest
    image: ghcr.io/alam00000/bentopdf-simple:latest
    container_name: bentopdf
    restart: unless-stopped
    ports:
      - "8080:8080"
```

Para iniciar o serviço, guarde o conteúdo num ficheiro `compose.yml` e execute:

```bash
docker compose up -d
```

Aceda a `http://IP_DO_SERVIDOR:8080` no navegador.

## BentoPDF Hyper Compress

[Repositório do BentoPDF Hyper Compress no GitHub](https://github.com/alam00000/bentopdf-hyper-compress)

Esta versão é focada na compressão de ficheiros PDF e permite configurar o tamanho máximo dos uploads e o número de tarefas simultâneas.

### Docker Compose

```yaml
services:
  hyper-compress:
    image: ghcr.io/alam00000/bentopdf-hyper-compress:latest
    container_name: hyper-compress
    restart: unless-stopped
    ports:
      - "8080:8080"
    environment:
      - HYPER_MAX_UPLOAD_MB=500
      - HYPER_CONCURRENCY=2
    tmpfs:
      - /tmp
```

Para iniciar o serviço, guarde o conteúdo num ficheiro `compose.yml` e execute:

```bash
docker compose up -d
```

Aceda a `http://IP_DO_SERVIDOR:8080` no navegador. Se o BentoPDF estiver a utilizar a porta `8080`, altere a porta do serviço Hyper Compress, por exemplo, para `8081:8080`.