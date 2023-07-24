# Stack de Serviços Docker - Aplicativo GLPI

Este repositório contém um playbook Docker Compose para implantar o aplicativo GLPI em um ambiente Docker Swarm como uma stack de serviços. A stack consiste em dois containers: um contêiner MySQL para armazenar o banco de dados do GLPI e um contêiner GLPI para executar o aplicativo. O playbook foi testado na versão 3.9 do Docker.

## Volumes

A stack utiliza dois volumes NFS:

- `glpi_site`: Este volume é utilizado para armazenar os arquivos do aplicativo GLPI.
- `glpi_db`: Este volume é utilizado para armazenar o banco de dados do GLPI.

## Serviços

### Contêiner MySQL

- Imagem: `mysql:latest`
- Hostname: `glpi_db`
- Volumes: O contêiner MySQL utiliza o volume `glpi_db` para armazenar o banco de dados do GLPI.
- Arquivo de ambiente: O arquivo `glpi_db.env` contém as variáveis de ambiente necessárias para configurar o contêiner MySQL.
- Implantação: O contêiner é implantado apenas em nós gerenciadores.
- Política de reinicialização: O contêiner é reiniciado em caso de falha.

### Contêiner GLPI

- Imagem: `diouxx/glpi`
- Hostname: `glpi`
- Portas: O contêiner GLPI expõe a porta 80, que pode ser mapeada para a porta desejada do host.
- Volumes: O contêiner GLPI utiliza o volume `glpi_site` para armazenar os arquivos do aplicativo GLPI. Ele também pode utilizar outros volumes conforme necessário.
- Variáveis de ambiente: As variáveis de ambiente necessárias para configurar o contêiner GLPI devem ser definidas no arquivo `glpi.env`.
- Dependências: Este contêiner depende do contêiner MySQL.
- Implantação: O contêiner é implantado apenas em nós trabalhadores.
- Política de reinicialização: O contêiner é reiniciado em caso de falha.

## Redes

A stack utiliza a rede `glpi_network` para conectar os contêineres MySQL e GLPI.

## Pré-requisitos

- Docker Swarm configurado em seu ambiente.
- Docker versão 3.9 ou superior.

## Instruções de Implantação

1. Certifique-se de ter o Docker Swarm configurado e em execução em seu ambiente.
2. Clone este repositório em sua máquina local.
3. Navegue até o diretório raiz do repositório clonado.
4. Execute o seguinte comando para implantar a stack de serviços: `docker stack deploy -c docker-compose.yml glpi`.
5. Aguarde alguns minutos para que os serviços sejam criados e iniciados.
6. Acesse o aplicativo GLPI em seu navegador utilizando o endereço correto.

## Configuração

Antes de implantar a stack de serviços, verifique se as configurações do banco de dados estão corretas no arquivo `glpi_db.env`. Ajuste as variáveis de ambiente necessárias no arquivo `glpi.env` de acordo com suas necessidades.

## Notas

- O aplicativo GLPI pode levar alguns minutos para inicializar na primeira execução
