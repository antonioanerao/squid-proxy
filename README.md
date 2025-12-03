# Squid Proxy descomplicado


<a href="https://github.com/antonioanerao/squid-proxy"><img alt="Squid Proxy" src="https://antonio.anerao.dev/wp-content/uploads/2024/06/curl-usando-squid.jpeg" width="100%"></a>

### Sobre o Squid Proxy
Squid é um proxy de cache que suporta HTTP, HTTPS, FTP, e mais. Ele é amplamente utilizado para melhorar o desempenho da rede e fornecer controle de acesso.

Outro exemplo de uso para um _proxy_ é fazer o deploy do serviço em um servidor nos Estados Unidos para acessar livremente o [https://claude.ai/](https://claude.ai/), que é bloqueado no Brasil.

### Sobre o [Traefik](https://antonio.anerao.dev/traefik-com-ssl-renovacao-automatica/)
Traefik é um roteador reverso moderno e dinâmico desenvolvido para fazer a integração com serviços de orquestração como Docker, Kubernetes, Consul, e outros. 

Neste projeto ele é utilizado para rotear o tráfego HTTP e HTTPS para os containers e gerenciar certificados SSL automaticamente.

#### Primeiros passos

Clone este repositório no seu servidor

```bash
git clone https://github.com/antonioanerao/squid-proxy.git
```

Acesse a pasta clonada

```bash
cd squid-proxy
```

Altere as permissões do arquivo ./data/acme.json para 600

```bash
sudo chmod 600 ./data/acme.json
```

Limpe todo o conteúdo do arquivo _passwords_

```bash
echo "" > ./passwords
```

Dê permissão de execução para o arquivo _novo-usuario.sh_ e execute-o para criar um novo usuário para autenticar no seu proxy. Você pode criar mais de um usuário.

```bash
chmod u+x ./novo-usuario.sh
./novo-usuario.sh meuNovoUsuario
```

Se aparecer uma mensagem dizendo que o programa htpasswd não foi encontrado, instale-o

```
sudo apt-get install apache2-utils
```

Abra o arquivo docker-compose.yml e, na linha 32, altere o _dominio.com.br_ para o domínio que você vai usar

Suba sua stack Traefik + Squid Proxy

```bash
docker compose up -d
```

Testando o retorno do seu proxy

```bash
curl -x seuUsuario:suaSenha@seudomini.com.br:9000 http://ip-api.com
```

#### NOTAS IMPORTANTES

Recebeu erro de htpasswd não instalado? Segue exemplo de instalação em uma distribuição linux baseada no Debian

```bash
sudo apt install apache2-utils
```

Ainda não tem o Docker instalado? Siga as instruções do site oficial em [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)

Adicionou um novo usuário? Reinicie a stack

```bash
docker compose down
docker compose up -d
```
