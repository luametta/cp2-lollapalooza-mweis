# 🎵 Lollapalooza API

API desenvolvida em Java com Spring Boot para gerenciamento de informações relacionadas ao festival Lollapalooza, incluindo dias do evento e palcos.

---

## 📌 Descrição do Projeto

Este projeto consiste em uma API REST que permite realizar operações de CRUD (Create, Read, Update, Delete) para as entidades:

* **Dia**: representa os dias do evento
* **Palco**: representa os palcos onde ocorrem os shows

A aplicação segue uma arquitetura padrão em camadas:

* **Controller**: responsável pelos endpoints da API
* **Model**: representa as entidades do sistema
* **Repository**: interface de acesso ao banco de dados

---

## 🚀 Tecnologias Utilizadas

* Java 17
* Spring Boot
* Maven
* Spring Data JPA
* Docker
* Swagger / OpenAPI
* Banco de dados relacional (H2 para desenvolvimento / MySQL/PostgreSQL configurável via profiles)

---

## 📂 Estrutura do Projeto

```text
src/main/java/br/fiap/cp1/lollapalooza
│
├── controller
│   ├── DiaController.java
│   └── PalcoController.java
│
├── model
│   ├── Dia.java
│   └── Palco.java
│
├── repository
│   ├── DiaRepository.java
│   └── PalcoRepository.java
│
└── LollaAPI.java
```

---

## ⚙️ Como Executar o Projeto

### Pré-requisitos

* Java 17+ (ou versão compatível)
* Maven instalado (ou usar o wrapper incluído)
* Docker (para execução via container)

### Execução Local (Profile Default)

1. Clone o repositório ou extraia o arquivo `.zip`
2. Acesse a pasta do projeto
3. Execute o comando:

```bash
./mvnw spring-boot:run
```

Ou no Windows:

```bash
mvnw.cmd spring-boot:run
```

4. A aplicação estará disponível em: `http://localhost:8080`

### Execução com Docker (Profile PRD)

Para executar a aplicação em produção utilizando a imagem publicada no Docker Hub, siga os passos abaixo. Este método atende aos requisitos de configuração por profiles e variáveis de ambiente.

**1. Baixar a imagem do Docker Hub:**

```bash
docker pull luametta/cp2-lollapalooza-mweis:latest
```

**2. Executar o container:**

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
*(Nota: `host.docker.internal` permite que o container acesse o banco de dados rodando na sua máquina local. Em ambientes Linux, pode ser necessário usar `--add-host=host.docker.internal:host-gateway`)*

---

## 🔑 Variáveis de Ambiente Necessárias

A aplicação utiliza variáveis de ambiente para configurar a conexão com o banco de dados, especialmente no profile `prd`:

| Variável | Descrição | Exemplo |
| :--- | :--- | :--- |
| `SPRING_PROFILES_ACTIVE` | Define o profile de execução (obrigatório `prd` para produção) | `prd` |
| `DB_SERVER_URL` | Endereço do servidor do banco de dados | `host.docker.internal` |
| `DB_SERVER_PORT` | Porta do banco de dados | `3306` |
| `DB_SCHEMA` | Nome do banco de dados (deve existir previamente) | `lollapalooza` |
| `DB_USER` | Usuário do banco de dados | `root` |
| `DB_PWD` | Senha do banco de dados | `root_pwd` |

> ⚠️ **Atenção sobre o profile `prd`**: Conforme os requisitos do projeto, o profile `prd` está configurado com `spring.jpa.hibernate.ddl-auto=none`. Isso significa que **o banco de dados e as tabelas devem ser criados manualmente antes da execução** (utilize o script `src/main/resources/migration.sql`). A aplicação não criará a estrutura do banco automaticamente neste modo.

---

## 📚 Documentação da API (Swagger / OpenAPI)

Com a aplicação em execução (local ou via Docker), a documentação interativa dos endpoints estará disponível em:

* **Swagger UI**: http://localhost:8080/swagger-ui.html
* **OpenAPI JSON**: http://localhost:8080/v3/api-docs

---

## 🔗 Endpoints Principais

### 📅 Dia

* `GET /dias` → Lista todos os dias
* `GET /dias/{id}` → Busca um dia por ID
* `POST /dias` → Cria um novo dia
* `PUT /dias/{id}` → Atualiza um dia
* `DELETE /dias/{id}` → Remove um dia

### 🎤 Palco

* `GET /palcos` → Lista todos os palcos
* `GET /palcos/{id}` → Busca um palco por ID
* `POST /palcos` → Cria um novo palco
* `PUT /palcos/{id}` → Atualiza um palco
* `DELETE /palcos/{id}` → Remove um palco

---

## 🧪 Testes

Os testes estão localizados em:

```text
src/test/java/br/fiap/cp1/lollapalooza
```

Para executar:

```bash
./mvnw test
```

---

## ⚙️ Configurações de Profiles

As configurações da aplicação são divididas por profiles para atender diferentes ambientes:

* **`application.properties` (default)**: Configurado para desenvolvimento. Cria o banco e as tabelas automaticamente (`ddl-auto=update`).
* **`application-prd.properties` (prd)**: Configurado para produção. **Não** cria banco/tabelas automaticamente (`ddl-auto=none`), exigindo que a estrutura já exista.

---

## 📦 Build do Projeto

Para gerar o `.jar` localmente:

```bash
./mvnw clean package
```

O arquivo será gerado em:

```text
target/lollapalooza-0.0.1-SNAPSHOT.jar
```

Para executar o `.jar` diretamente:

```bash
java -jar target/lollapalooza-0.0.1-SNAPSHOT.jar
```

---

## 👨‍💻 Autores

Projeto acadêmico desenvolvido para estudo de APIs REST com Spring Boot, Microservices e Web Engineering.

* **Luana Metta Ribeiro Fernandes** - RM: 558314
* **Luísa Souza Santos** - RM: 557799

---

## 📄 Licença

Este projeto é de uso educacional.

---
