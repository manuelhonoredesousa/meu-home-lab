# Odysseus

O Odysseus é um ambiente de IA auto-hospedado para trabalhar com ferramentas locais e tarefas de automação em rede.

## Repositório

- [odysseus-dev/odysseus no GitHub](https://github.com/odysseus-dev/odysseus)

## Instalação rápida

```bash
git clone https://github.com/odysseus-dev/odysseus.git
cd odysseus
cp .env.example .env
nano .env
```

No ficheiro `.env`, configure as variáveis de rede e autenticação:

```env
APP_BIND=0.0.0.0
ODYSSEUS_ADMIN_USER=admin
ODYSSEUS_ADMIN_PASSWORD=EscolhaUmaSenhaForteAqui
```

> `APP_BIND=0.0.0.0` permite que o serviço seja acessível a partir de outros computadores da rede local.

Depois, inicie o ambiente:

```bash
docker compose up -d --build
```

## Se houver problema ao iniciar sessão

Se o login falhar ou a autenticação ficar corrompida, remova o ficheiro de autenticação e reinicie o serviço:

```bash
rm -f data/auth.json
docker compose down && docker compose up -d --build
```

## Acesso

Depois da inicialização _(ex: 192.168.0.2:7000)_, abra o endereço do servidor no navegador, normalmente no IP da máquina seguido da porta configurada pela aplicação.
