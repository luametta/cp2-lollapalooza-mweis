# LollapaloozaApiCp2

API REST desenvolvida em Spring Boot para gerenciamento de dias do evento e palcos do festival Lollapalooza, com acesso a banco de dados MySQL, documentacao Swagger/OpenAPI, configuracao por profiles e execucao com Docker.

Check Point 1 — Microservices and Web Engineering — Prof. Antonio Carlos de Lima Junior.

Repositorio GitHub: https://github.com/luametta/cp2-lollapalooza-mweis
Repositorio Docker Hub: https://hub.docker.com/r/luametta/cp2-lollapalooza-mweis

## Pre-requisitos

Para executar o projeto localmente, voce vai precisar ter instalado:

- Java 17
- Maven
- MySQL
- Docker (opcional)

## Execucao local

### 1. Configuracao das variaveis de ambiente

A aplicacao utiliza variaveis de ambiente para configurar a conexao com o banco de dados e o profile do Spring Boot.

| Variavel | Descricao | Exemplo |
| :--- | :--- | :--- |
| `DB_SERVER_URL` | Endereco do servidor do banco de dados | `localhost` |
| `DB_SERVER_PORT` | Porta do banco de dados | `3306` |
| `DB_SCHEMA` | Nome do schema | `lollapalooza` |
| `DB_USER` | Usuario do banco de dados | `root` |
| `DB_PWD` | Senha do banco de dados | `root_pwd` |
| `SPRING_PROFILES_ACTIVE` | Profile ativo do Spring Boot | `default` |

No profile default, se as variaveis nao forem definidas, a aplicacao usa valores padrao (localhost, 3306, lollapalooza, root, root_pwd).

**Linux / macOS**
```bash
export DB_SERVER_URL=localhost
export DB_SERVER_PORT=3306
export DB_SCHEMA=lollapalooza
export DB_USER=root
export DB_PWD=root_pwd
export SPRING_PROFILES_ACTIVE=default
```

**Windows PowerShell**
```powershell
$env:DB_SERVER_URL="localhost"
$env:DB_SERVER_PORT="3306"
$env:DB_SCHEMA="lollapalooza"
$env:DB_USER="root"
$env:DB_PWD="root_pwd"
$env:SPRING_PROFILES_ACTIVE="default"
```

### 2. Executar a aplicacao

Com Maven:

```bash
mvn spring-boot:run
```

Ou utilizando o Maven Wrapper:

```bash
./mvnw spring-boot:run
```

No Windows:

```cmd
.\mvnw.cmd spring-boot:run
```

A aplicacao sera iniciada em:

http://localhost:8080

## Execucao com Docker

### 1. Baixar a imagem do Docker Hub

```bash
docker pull luametta/cp2-lollapalooza-mweis:latest
```

Tambem esta disponivel a tag `1.0.0`:

```bash
docker pull luametta/cp2-lollapalooza-mweis:1.0.0
```

Ou, para gerar a imagem localmente a partir do codigo-fonte:

```bash
docker build -t cp2-lollapalooza-mweis:1.0.0 .
```

### 2. Executar o container

Caso o banco de dados esteja sendo executado na maquina host, utilize `host.docker.internal` para permitir que o container acesse o banco.

```bash
docker run -d \
  --name lollapalooza-api \
  -p 8080:8080 \
  -e DB_SERVER_URL=host.docker.internal \
  -e DB_SERVER_PORT=3306 \
  -e DB_SCHEMA=lollapalooza \
  -e DB_USER=root \
  -e DB_PWD=root_pwd \
  -e SPRING_PROFILES_ACTIVE=default \
  luametta/cp2-lollapalooza-mweis:latest
```

No Windows PowerShell:

```powershell
docker run -d `
  --name lollapalooza-api `
  -p 8080:8080 `
  -e DB_SERVER_URL=host.docker.internal `
  -e DB_SERVER_PORT=3306 `
  -e DB_SCHEMA=lollapalooza `
  -e DB_USER=root `
  -e DB_PWD=root_pwd `
  -e SPRING_PROFILES_ACTIVE=default `
  luametta/cp2-lollapalooza-mweis:latest
```

A aplicacao ficara disponivel em:

http://localhost:8080

*Nota: `host.docker.internal` permite que o container acesse servicos executados na maquina host. Em ambientes Linux, dependendo da configuracao do Docker, pode ser necessario utilizar uma configuracao de rede diferente (ex.: `--add-host=host.docker.internal:host-gateway`), ou apontar `DB_SERVER_URL` para o nome do container do MySQL caso ambos estejam na mesma rede Docker (`docker network create` + `--network`).*

### 3. Validar

```bash
curl http://localhost:8080/dias
```

## Profiles do Spring Boot

O profile ativo da aplicacao e definido atraves da variavel de ambiente `SPRING_PROFILES_ACTIVE`. A aplicacao possui dois profiles:

### `default`

Profile padrao, usado quando `SPRING_PROFILES_ACTIVE` nao e definida ou e definida como `default`. Configurado em `src/main/resources/application.properties`.

- Cria o banco de dados automaticamente, se ele nao existir (`createDatabaseIfNotExist=true`).
- Cria/atualiza as tabelas automaticamente (`spring.jpa.hibernate.ddl-auto=update`).
- `spring.jpa.show-sql=true`.

```bash
export SPRING_PROFILES_ACTIVE=default
```

### `prd`

Profile de producao, configurado em `src/main/resources/application-prd.properties`.

- Nao cria o banco de dados nem as tabelas automaticamente (`spring.jpa.hibernate.ddl-auto=none`, sem `createDatabaseIfNotExist`).
- O banco e as tabelas precisam existir antes de a aplicacao subir — use o script `src/main/resources/migration.sql`.
- `spring.jpa.show-sql=false`.
- Todas as variaveis de conexao (`DB_SERVER_URL`, `DB_SERVER_PORT`, `DB_SCHEMA`, `DB_USER`, `DB_PWD`) sao obrigatorias, sem valor padrao.

```bash
export SPRING_PROFILES_ACTIVE=prd
```

Ao executar com Docker:

```bash
docker run -d \
  --name lollapalooza-api \
  -p 8080:8080 \
  -e SPRING_PROFILES_ACTIVE=prd \
  -e DB_SERVER_URL=host.docker.internal \
  -e DB_SERVER_PORT=3306 \
  -e DB_SCHEMA=lollapalooza \
  -e DB_USER=root \
  -e DB_PWD=root_pwd \
  luametta/cp2-lollapalooza-mweis:latest
```

### Criando o schema para o profile `prd`

Antes de subir a aplicacao no profile `prd`, aplique o script de criacao das tabelas em um MySQL acessivel:

```bash
mysql -h <host> -P <porta> -u <usuario> -p < src/main/resources/migration.sql
```

Ou, se o MySQL estiver rodando em um container:

```bash
docker exec -i <container_mysql> mysql -uroot -p<senha> < src/main/resources/migration.sql
```

## Swagger / OpenAPI

Com a aplicacao em execucao (qualquer profile), a documentacao Swagger fica disponivel na raiz:

http://localhost:8080/swagger-ui.html

Especificacao OpenAPI (JSON):

http://localhost:8080/v3/api-docs

## Endpoints da API

### `/dias`

| Metodo | Rota | Descricao |
| :--- | :--- | :--- |
| POST | `/dias` | Cadastra um dia |
| GET | `/dias` | Lista todos os dias |
| GET | `/dias/{id}` | Busca um dia por id |
| PUT | `/dias/{id}` | Atualiza um dia |
| DELETE | `/dias/{id}` | Remove um dia |

```json
// POST /dias
{
  "id": 1,
  "data": "2026-03-20"
}
```

### `/palcos`

| Metodo | Rota | Descricao |
| :--- | :--- | :--- |
| POST | `/palcos` | Cadastra um palco |
| GET | `/palcos` | Lista todos os palcos |
| GET | `/palcos/{id}` | Busca um palco por id |
| PUT | `/palcos/{id}` | Atualiza um palco |
| DELETE | `/palcos/{id}` | Remove um palco |

```json
// POST /palcos
{
  "id": 1,
  "nome": "Palco Mundo",
  "capacidade": 100000
}
```

## Docker — comandos uteis

### Criar a imagem
```bash
docker build -t cp2-lollapalooza-mweis:1.0.0 .
```

### Executar o container
```bash
docker run -d \
  --name lollapalooza-api \
  -p 8080:8080 \
  -e DB_SERVER_URL=host.docker.internal \
  -e DB_SERVER_PORT=3306 \
  -e DB_SCHEMA=lollapalooza \
  -e DB_USER=root \
  -e DB_PWD=root_pwd \
  -e SPRING_PROFILES_ACTIVE=default \
  cp2-lollapalooza-mweis:1.0.0
```

### Publicar no Docker Hub
```bash
docker login
docker tag cp2-lollapalooza-mweis:1.0.0 luametta/cp2-lollapalooza-mweis:1.0.0
docker push luametta/cp2-lollapalooza-mweis:1.0.0
```

### Listar containers em execucao
```bash
docker ps
```

### Parar e remover o container
```bash
docker stop lollapalooza-api
docker rm lollapalooza-api
```

### Ver logs
```bash
docker logs -f lollapalooza-api
```

## Seguranca

Nao versione credenciais reais no repositorio.

Recomenda-se utilizar um arquivo `.env` local para desenvolvimento e adiciona-lo ao `.gitignore`:

```
.env
```

Exemplo de conteudo:

```env
DB_SERVER_URL=localhost
DB_SERVER_PORT=3306
DB_SCHEMA=lollapalooza
DB_USER=root
DB_PWD=root_pwd
SPRING_PROFILES_ACTIVE=default
```
```
