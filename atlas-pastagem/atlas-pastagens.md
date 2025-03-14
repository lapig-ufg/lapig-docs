# HOME

## Documentação Atlas das Pastagens

O Atlas das Pastagens, público e gratuito, foi desenvolvido pelo Laboratório de Processamento de Imagens e Geoprocessamento da Universidade Federal de Goiás (Lapig/UFG), para tornar acessível os resultados e produtos, gerados no âmbito da iniciativa MapBiomas, referentes às pastagens brasileiras.

Essa é uma documentação para a plataforma [Atlas das Pastagens](https://atlasdaspastagens.ufg.br/), desenvolvida pelo [Laboratório de Processamento de Images e Geoprocessamento da Universidade Federal de Goiás (LAPIG/UFG)](https://www.lapig.iesa.ufg.br/).

Exemplo: [URL](https://lapig-ufg.github.io/d-pat/)

## Dados Geográficos

Dados Geográficos...

# ARQUITETURA DE SOFTWARE

## Visão Geral

A arquitetura de software desenvolvida para o Cerrado DPAT pode ser observada na Figura abaixo.

![Sample Image](./assets/images/componentes.svg)

Essencialmente, pode-se dividir o Atlas das Pastagens em três partes:

- **Client**: Responsável por apresentar toda a interface, construído em Angular 16 com apoio dos frameworks:

    - OpenLayers: Exibir e manipular mapas interativos.

    - PrimeNG: Componentes visuais (UI).

- **Server**: Disponibiliza os serviços que compõem a aplicação:

    - Application Server: Disponibiliza o acesso aos dados do Data Source, análises e configurações.

    - Assets Server: Disponibiliza o acesso aos assets que compõe

    - Storage Server: 

    - OWS Server: Disponibiliza o acesso ao MapServer, responsável pelo processamento dos dados geográficos, apresentação visual dos mesmos e gerenciamento de cache das imagens geradas.

- **Data Source**: Responsável por armazenar os dados vetoriais e matriciais.

Resumidamente, todos os dados usados pelo Atlas das Pastagens estão organizados em um banco de dados, PostgreSQL e MongoDB, e arquivos de repositórios gerenciados pelo MINIO. O PostgreSQL utiliza a extensão PostGIS que permite processar consultas espaciais via Structured Query Language (SQL). Já os dados que não necessitam de cruzamentos espaciais são armazenados em arquivos nos formatos Shapefile e/ou GeoTIFF.

Estes dados são acessados pelo Application Server e pelo OWS Server (ambos construídos em NodeJS) e são disponibilizados através de URLs com requisições por meio do protocolo HTTP. Todas as requisições Web realizadas aos Servers são interceptadas pelo Apache Server e devidamente redirecionadas. Desta forma, o Client realiza todas as requisições necessárias para construir a visualização dos elementos da página, tais como: mapa interativo, gráficos, campo de busca e etc.

Por fim, destaca-se que todo o código-fonte para o Cerrado DPAT e também para o OWS Server foi disponibilizado publicamente no Github do LAPIG/UFG.

### Componentes de Software

## Servidor de Mapas

### Serviço de Interoperabilidade

### Provesso de Atualização

### Disponibilidade da Camada no Application Server

## Servidor de Aplicação

### Middleware para Manipulação do Banco de Dados

### Como Criar um Novo Serviço
