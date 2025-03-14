Exemplo: [URL](https://lapig-ufg.github.io/d-pat/)

# HOME

## Documentação Atlas das Pastagens

O Atlas das Pastagens, público e gratuito, foi desenvolvido pelo Laboratório de Processamento de Imagens e Geoprocessamento da Universidade Federal de Goiás (Lapig/UFG), para tornar acessível os resultados e produtos, gerados no âmbito da iniciativa MapBiomas, referentes às pastagens brasileiras.

Essa é uma documentação para a plataforma [Atlas das Pastagens](https://atlasdaspastagens.ufg.br/), desenvolvida pelo [Laboratório de Processamento de Images e Geoprocessamento da Universidade Federal de Goiás (LAPIG/UFG)](https://www.lapig.iesa.ufg.br/).

## Dados Geográficos

Dados Geográficos...

# ARQUITETURA DE SOFTWARE

## Visão Geral

A arquitetura de software desenvolvida para o Atlas das Pastagens pode ser observada na Figura abaixo.

### Componentes de Software

A arquitetura de software desenvolvida para o Atlas das Pastagens pode ser observada na Figura abaixo.

![Sample Image](./assets/images/componentes.svg)

Essencialmente, pode-se dividir o Atlas das Pastagens em três partes:

- **Client**: Responsável por apresentar toda a interface, construído em Angular 16 com apoio dos frameworks:

    - OpenLayers: Exibir e manipular mapas interativos.

    - PrimeNG: Componentes visuais (UI).

- **Server**: Disponibiliza os serviços que compõem a aplicação:

    - Application Server: Disponibiliza o acesso aos dados do Data Source, análises e configurações.

    - Assets Server: Disponibiliza o acesso aos assets que compõe

    - Storage Server: 

    - [OWS Server](../ows-server/ows-server.md): Disponibiliza o acesso ao MapServer, responsável pelo processamento dos dados geográficos, apresentação visual dos mesmos e gerenciamento de cache das imagens geradas.

- **Data Source**: Responsável por armazenar os dados vetoriais e matriciais.

Resumidamente, todos os dados usados pelo Atlas das Pastagens estão organizados em bancos de dados, PostgreSQL e MongoDB, e arquivos de repositórios gerenciados pelo MINIO. O PostgreSQL utiliza a extensão PostGIS que permite processar consultas espaciais via Structured Query Language (SQL). Já os dados que não necessitam de cruzamentos espaciais são armazenados em arquivos nos formatos Shapefile e/ou GeoTIFF. Assets utilizados pelo hotsite, como fotos de perfil e descrições da equipe, são armazenados pelo MINIO e manipulados pelo Voyager.

Estes dados são acessados pelo Application Server e pelo MINIO e são disponibilizados através de URLs com requisições por meio do protocolo HTTP. Todas as requisições Web realizadas aos Servers são interceptadas pelo Apache Server e devidamente redirecionadas. Desta forma, o Client realiza todas as requisições necessárias para construir a visualização dos elementos da página, tais como: mapa interativo, gráficos, campo de busca e etc.

Por fim, destaca-se que todos o código-fontes foram disponibilizado publicamente no Github do LAPIG/UFG.

## Servidor de Aplicação

Basicamente o Atlas das Pastagens é dividido em duas partes: o WebMap Client e o Application Server, conforme descrito anteriormente. O código-fonte para estas duas partes está hospedado no respositório Github do projeto.

Dentro do repositório do projeto, o código-fonte do Application Server está em src/server. Por ser construído em NodeJS, ao clonar o projeto o usuário deverá navegar até a pasta server e instalar as dependências gerenciadas pelo Node Package Manager (NPM) através do comando:

npm install
Em seguida, deve-se copiar o arquivo .env.exemple renomeando-o para .env:

cp .env.exemple .env
Em seguida, deve-se alterar o arquivo .env com as devidas configurações e parâmetros referentes ao banco de dados, pastas para armazenamento dos arquivos de Upload e Download e endereço de hospedagem do OWS Server. Um exemplo de arquivo .env pode ser observado na seção

Por fim, a fim de facilitar a execução do Application Server foi desenvolvido um script nomeado start.sh localizado na raiz da pasta src/server. Portanto, basta realizar a execução deste arquivo para inicializar o Application Server. A fim de identificar modificações em tempo real, o Application Server faz uso da biblioteca always, portanto talvez seja necessário a instalação da mesma através do comando.

### Provesso de Atualização

### Disponibilidade da Camada no Application Server

## Servidor de Aplicação

### Middleware para Manipulação do Banco de Dados

### Como Criar um Novo Serviço
