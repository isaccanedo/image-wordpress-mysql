# Configuração do Ambiente WordPress com Docker Compose

Este repositório contém uma configuração para iniciar um ambiente de desenvolvimento local do **WordPress** e **MySQL** usando o **Docker Compose**.  
Isso permite que você tenha um ambiente de trabalho consistente e isolado, sem a necessidade de instalar o servidor web e o banco de dados diretamente em sua máquina.

---

## Pré-requisitos

Para usar esta configuração, você precisa ter o **Docker** e o **Docker Compose** instalados em seu sistema.

- [Instalação do Docker](https://docs.docker.com/get-docker/)  
- [Instalação do Docker Compose](https://docs.docker.com/compose/install/)

---

## Estrutura do Projeto

A estrutura de diretórios esperada é a seguinte:

```
.
├── docker-compose.yml
└── wp-content/
```

```
- **docker-compose.yml**: Arquivo principal que define os serviços (contêineres), redes e volumes.  
- **wp-content/**: Diretório local mapeado para `/var/www/html/wp-content` dentro do contêiner do WordPress.  
  - Temas, plugins e uploads adicionados serão persistidos mesmo que o contêiner seja removido.

---

## Como Usar

### 1. Clonar o Repositório

```bash
git clone <URL_DO_SEU_REPOSITORIO>
cd <nome_do_seu_diretorio>
```

### 2. Iniciar os Contêineres

```
docker compose up -d
```

docker compose up: Lê o docker-compose.yml, constrói as imagens (se necessário) e inicia os serviços.

-d: Executa os contêineres em detached mode (segundo plano).

Na primeira execução, o Docker fará o download das imagens mysql:8.0 e wordpress:latest.

### 3. Acessar o WordPress

Abra no navegador:

```
http://localhost:8080
```

Na primeira vez, você será redirecionado para a página de instalação do WordPress.

### 4. Gerenciar o Ambiente

Verificar status dos contêineres:

```
docker compose ps

```

Parar os contêineres:

```
docker compose down

```

Este comando remove contêineres e rede, mas mantém volumes e dados.

Parar e remover todos os dados (incluindo volumes):

```
docker compose down --volumes

```

⚠️ Atenção: Esse comando remove permanentemente os dados do banco (db_data) e arquivos do WordPress (wp-content).

Detalhes da Configuração
```
Serviços
db

Imagem: mysql:8.0

container_name: wordpress-db

restart: always

environment: Variáveis de ambiente para configurar o banco de dados.

volumes: db_data para persistência dos dados.

wordpress

Imagem: wordpress:latest

container_name: wordpress-app

restart: always

ports: 8080:80

environment: Variáveis para conectar ao banco (WORDPRESS_DB_HOST=db).

depends_on:

db (garante que o banco suba primeiro)

volumes: ./wp-content:/var/www/html/wp-content

Volumes

db_data: Volume nomeado gerenciado pelo Docker para persistência do MySQL.
```

```
docker volume ls

```
