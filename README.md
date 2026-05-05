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

* Java
* Spring Boot
* Maven
* Spring Data JPA
* H2 / Banco configurável via `application.properties`

---

## 📂 Estrutura do Projeto

```
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

### Passos

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

4. A aplicação estará disponível em:

```
http://localhost:8080
```

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

```
src/test/java/br/fiap/cp1/lollapalooza
```

Para executar:

```bash
./mvnw test
```

---

## ⚙️ Configurações

As configurações da aplicação podem ser alteradas no arquivo:

```
src/main/resources/application.properties
```

---

## 📦 Build do Projeto

Para gerar o `.jar`:

```bash
./mvnw clean package
```

O arquivo será gerado em:

```
target/lollapalooza-0.0.1-SNAPSHOT.jar
```

Para executar o `.jar`:

```bash
java -jar target/lollapalooza-0.0.1-SNAPSHOT.jar
```

---

## 👨‍💻 Autor

Projeto acadêmico desenvolvido para estudo de APIs REST com Spring Boot.

---

## 📄 Licença

Este projeto é de uso educacional.
