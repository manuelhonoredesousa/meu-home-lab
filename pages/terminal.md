# Terminal e comandos úteis

Esta página serve como referência rápida para tarefas frequentes no Ubuntu Server e no ambiente de home lab.

## 1. Gestão de ficheiros e pastas

```bash
mkdir sousa
mkdir -p manuel/bunga/honore
mkdir -p a/{man,mon}

ls -ld sousa
ls -la

find . -maxdepth 2 -type d
find /home -type f | head

du -sh sousa
du -sh .

touch arquivo.txt
cp arquivo.txt backup.txt
mv backup.txt /tmp/
cp -r manuel /tmp/

rmdir sousa
rm -rf sousa
```

### O que estes comandos fazem

- `mkdir`: cria pastas
- `mkdir -p`: cria diretórios intermédios automaticamente
- `touch`: cria um ficheiro vazio
- `cp`: copia ficheiros ou pastas
- `mv`: move ou renomeia ficheiros
- `find`: procura ficheiros e pastas por critérios
- `ls -la`: mostra ficheiros ocultos e detalhes
- `du -sh`: indica o tamanho de uma pasta
- `rmdir`: remove uma pasta vazia
- `rm -rf`: remove ficheiros ou diretórios forçadamente

> Tenha cuidado com `rm -rf`, porque elimina a pasta sem confirmação.

### Exemplos práticos

```bash
mkdir -p /srv/docker/nextcloud
cp /etc/hosts /tmp/hosts.backup
mv /tmp/hosts.backup /home/sousa/
```

Estes comandos são úteis para organizar ficheiros de configuração, dados de aplicações e backups simples.

## 2. Informação do sistema

```bash
uname -a
cat /etc/os-release
whoami
hostname
hostname -I
uptime
```

Esses comandos ajudam a confirmar:

- a distribuição Linux em uso
- o nome da máquina
- o IP do servidor
- o tempo de atividade do sistema

## 3. Redes e diagnóstico

```bash
ip a
ip route
ping 8.8.8.8
ping www.google.com
ss -tulpn
netstat -tulpn
```

Use estes comandos para:

- ver interfaces de rede
- confirmar gateway e rotas
- testar conectividade com a internet
- verificar portas abertas e serviços a escutar

## 4. Atualizações e manutenção

```bash
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
sudo apt install curl wget htop git -y
```

Atualizações regulares ajudam a manter o servidor estável e seguro.

## 5. Gestão de serviços

```bash
sudo systemctl status ssh
sudo systemctl start ssh
sudo systemctl enable ssh
sudo systemctl restart ssh
sudo systemctl stop ssh
```

Os serviços podem ser controlados facilmente com `systemctl`, que é útil para:

- verificar se um serviço está em execução
- iniciar ou reiniciar um serviço
- ativar um serviço no arranque do sistema

## 6. Reboot e shutdown

```bash
sudo reboot
sudo shutdown now
sudo shutdown -r +10
```

Use `reboot` para reiniciar e `shutdown` para desligar de forma controlada.

## 7. Permissões e proprietários

```bash
ls -l
ls -ld /var/www
chmod 755 script.sh
chmod 600 arquivo.txt
chmod 644 arquivo.txt
chmod +x script.sh
chown usuario:usuario pasta
chown -R usuario:usuario /srv/docker

stat arquivo.txt
```

### Permissões comuns

- `755`: executável para o dono, leitura e execução para os outros
- `600`: apenas o dono pode ler e escrever
- `644`: leitura para todos, escrita apenas para o dono
- `700`: o dono tem acesso completo; outros não têm acesso

### Explicação rápida

Cada ficheiro ou pasta tem:

- dono (`user`)
- grupo (`group`)
- permissões para `owner`, `group` e `others`

O formato normal é algo como:

```bash
-rw-r--r--
```

Que significa:

- `-` = ficheiro regular
- `rw-` = dono lê e escreve
- `r--` = grupo lê
- `r--` = outros lêem

### Exemplos úteis

```bash
sudo chown www-data:www-data /var/www/html
sudo chmod 755 /var/www/html
sudo chmod -R 750 /srv/backup
````

Use `chmod` para controlar quem pode ler, escrever ou executar um ficheiro. Em home lab, isto é importante para pastas de Docker, servidores web, backups e ficheiros de configuração.

### Bit especial: `setfacl` e `umask`

```bash
umask
sudo setfacl -m u:usuario:rwx /pasta
getfacl /pasta
```

Estas opções são úteis quando a configuração de permissões simples não é suficiente, por exemplo em pastas partilhadas entre utilizadores ou serviços.

## 8. Logs e resolução de problemas

```bash
journalctl -xe
journalctl -u ssh
sudo tail -f /var/log/syslog
```

Esses comandos ajudam a ver erros do sistema e verificar o comportamento de serviços.

## 9. Docker (muito útil em home lab)

```bash
docker ps
sudo docker ps -a
docker images
docker logs nome_do_container
docker compose up -d
docker compose down
```

O Docker é ideal para instalar serviços como:

- Nextcloud
- Home Assistant
- Portainer
- bases de dados
- aplicações de IA e automação

## 10. Segurança básica

```bash
sudo ufw status
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

Boas medidas incluem:

- ativar firewall
- usar SSH com chaves em vez de password
- manter o sistema atualizado
- não expor portas desnecessárias na rede

## 11. Dicas práticas

- use `sudo` apenas para comandos administrativos
- documente IP, portas, credenciais e versões de cada serviço
- teste alterações em ambiente de produção com cuidado
- mantenha backups antes de instalar serviços ou alterar dados críticos

## 12. Comando útil para documentação do home lab

```bash
hostnamectl
ip addr show
lsblk
df -h
```

 Estes quatro comandos ajudam a registar a configuração base do servidor e a manter a documentação organizada.
