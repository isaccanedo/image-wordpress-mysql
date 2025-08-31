Configuração do Ambiente WordPress com Docker Compose
Este repositório contém uma configuração para iniciar um ambiente de desenvolvimento local do WordPress e MySQL usando o Docker Compose. Isso permite que você tenha um ambiente de trabalho consistente e isolado, sem a necessidade de instalar o servidor web e o banco de dados diretamente em sua máquina.

Pré-requisitos
Para usar esta configuração, você precisa ter o Docker e o Docker Compose instalados em seu sistema.

Instalação do Docker

Instalação do Docker Compose

Estrutura do Projeto
A estrutura de diretórios esperada é a seguinte:

.
├── docker-compose.yml
└── wp-content/

docker-compose.yml: Este é o arquivo principal que define os serviços (contêineres), redes e volumes. Ele orquestra a criação e a comunicação entre o contêiner do WordPress e o contêiner do MySQL.

wp-content/: Este diretório local será mapeado para o diretório /var/www/html/wp-content dentro do contêiner do WordPress. Isso significa que temas, plugins e uploads que você adicionar ao WordPress serão salvos neste diretório em sua máquina, persistindo mesmo que o contêiner seja removido.

Como Usar
1. Clonar o Repositório
Primeiro, clone este repositório para o seu ambiente local:

git clone <URL_DO_SEU_REPOSITORIO>
cd <nome_do_seu_diretorio>

2. Iniciar os Contêineres
Para iniciar o ambiente, navegue até o diretório onde o arquivo docker-compose.yml está localizado e execute o seguinte comando:

docker compose up -d

docker compose up: Este comando lê o docker-compose.yml, constrói as imagens (se necessário) e inicia os serviços.

-d: A flag -d significa "detached mode" (modo separado), que executa os contêineres em segundo plano, liberando seu terminal.

A primeira vez que você executar este comando, o Docker fará o download das imagens mysql:8.0 e wordpress:latest. Isso pode levar alguns minutos, dependendo da sua conexão com a internet.

3. Acessar o WordPress
Após os contêineres estarem em execução, você pode acessar o WordPress em seu navegador através do seguinte endereço:

http://localhost:8080

Na primeira vez, você será redirecionado para a página de instalação do WordPress, onde poderá configurar o site (idioma, título, nome de usuário e senha do administrador).

4. Gerenciar o Ambiente
Verificar o status dos contêineres:

docker compose ps

Parar os contêineres:

docker compose down

Este comando irá parar e remover os contêineres e a rede criada pelo Docker Compose. Os dados do banco de dados e os arquivos do WordPress em wp-content serão preservados porque estão associados a volumes.

Parar e remover todos os dados (incluindo volumes):

docker compose down --volumes

Use este comando com cautela, pois ele removerá o volume db_data e wp-content, apagando permanentemente todos os dados do banco de dados e arquivos do WordPress.

Detalhes da Configuração
Serviços
db:

Imagem: mysql:8.0, uma imagem oficial do MySQL.

container_name: wordpress-db para fácil identificação.

restart: always: O contêiner será reiniciado automaticamente se parar.

environment: Variáveis de ambiente para configurar o banco de dados. Os valores aqui devem ser os mesmos usados pelo serviço do WordPress para que eles possam se comunicar.

volumes: O volume db_data é usado para persistir os dados do banco de dados. Ele garante que os dados não sejam perdidos quando o contêiner for parado ou reiniciado.

wordpress:

Imagem: wordpress:latest, a versão mais recente do WordPress.

container_name: wordpress-app.

restart: always: Garante que a aplicação WordPress esteja sempre rodando.

ports: Mapeia a porta 80 do contêiner para a porta 8080 da sua máquina local, permitindo que você acesse a aplicação.

environment: Variáveis de ambiente para conectar o WordPress ao banco de dados. Note que WORDPRESS_DB_HOST usa o nome do serviço (db), pois o Docker Compose cria uma rede interna onde os serviços podem se referenciar uns aos outros pelo nome.

depends_on: - db: Garante que o serviço db (banco de dados) seja iniciado antes do serviço wordpress.

volumes: O diretório local ./wp-content é mapeado para o diretório wp-content do contêiner, permitindo que temas, plugins e mídias sejam persistidos.

Volumes
db_data: Um volume nomeado que o Docker gerencia para persistir os dados do banco de dados MySQL. Isso é crucial para que seus dados não sejam perdidos. Você pode inspecionar o volume com docker volume ls.
