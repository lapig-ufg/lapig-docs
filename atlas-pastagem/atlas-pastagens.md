Exemplo: [URL](https://lapig-ufg.github.io/d-pat/)

# HOME

## Documentação Atlas das Pastagens

O Atlas das Pastagens, público e gratuito, foi desenvolvido pelo Laboratório de Processamento de Imagens e Geoprocessamento da Universidade Federal de Goiás (Lapig/UFG), para tornar acessível os resultados e produtos, gerados no âmbito da iniciativa MapBiomas, referentes às pastagens brasileiras.

Essa é uma documentação para a plataforma [Atlas das Pastagens](https://atlasdaspastagens.ufg.br/), desenvolvida pelo [Laboratório de Processamento de Images e Geoprocessamento da Universidade Federal de Goiás (LAPIG/UFG)](https://www.lapig.iesa.ufg.br/).

## Dados Geográficos

Dados Geográficos...

### Pastagem

### Vigor de Pastagem

### Referência Pastagem

### Estoque de Carbono

### Visita de Campo Goiás

### Visita de Campo Amazonia Portal

### Visita de Campo Mato Grosso

### Visita de Campo Pontos Antigos

### CIV Mapbiomas col.3

### CIV Mapbiomas Classes Raras

### CIV Mapbiomas col.6

### Pecuária Censitária - IBGE

### Floresta Plantada - Mapbiomas

### Lavouras

### CAR - Prop. Rurais

### Reserva Legal

### Planejamento Hídrico

### Malha Viária

### Armazéns

### Frigorifios

### Terrars Indigenas

### Assentamentos

### Comunidades Quilombolas

### Unidades de Conservação

### Áreas Militares

### Áreas Públicas Não-Destinadas

### Satélite

## Métricas de Qualidade

Métricas de qualidade...

### Processos de Validação

#### Campo

Durante a vigência do projeto...

#### Validação

Duas superfícies de susceptibilidade...

### Análise Automáticas

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

Basicamente o Atlas das Pastagens é dividido em duas partes: Client e Application Server, conforme descrito anteriormente. O código-fonte para estas duas partes está hospedado no respositório Github do projeto.

Dentro do repositório do projeto, o código-fonte do Application Server está no diretório `./server`. Por ser construído em NodeJS, ao clonar o projeto o usuário deverá navegar até a pasta server e instalar as dependências gerenciadas pelo Node Package Manager (NPM) através do comando:

```shell
npm install
```

Em seguida, deve-se copiar o arquivo .env.exemple renomeando-o para .env:

```shell
cp .env.exemple .env
```

Em seguida, deve-se alterar o arquivo .env com as devidas configurações e parâmetros referentes ao banco de dados, pastas para armazenamento dos arquivos de Upload e Download e endereço de hospedagem do OWS Server.

Deve-se ainda, criar as pastas de upload com os seguintes comandos:

```shell
mkdir -p /home/tiago/Documentos/LAPIG/atlas-pastagens/server/plataforms_files/atlas-pastagens-files/uploads/
```

A fim de facilitar a execução do Application Server foi desenvolvido um script nomeado `start.sh` localizado na raiz da pasta `./server`. Portanto, basta realizar a execução deste arquivo para inicializar o Application Server. A fim de identificar modificações em tempo real, o Application Server faz uso da biblioteca `always`, portanto talvez seja necessário a instalação da mesma através do comando.

```shell
sudo npm install always -g
```

Após a instalação do always, pode-se inicializar o Application Server através do comando:

```shell
./src/server/start.sh
```

### Middleware para manipulação do Banco de Dados

Para facilitar a criação de serviços que fazem uso de consultas ao banco de dados foi criado um middleware nomeado `dataInjector.js` que cria um pool de conexões para execução de diversas queries simultâneas.

O `dataInjector.js` analisa a URL da requisição HTTP de modo a associar a função que especifica a query (por meio da tupla {id, sql}) com o controlador que irá devolver a resposta da requisição.

Portanto, todas as URLs deverão ser criadas no seguinte padrão:

```javascript
'/service/<nome_arquivo_js_com_query>/<nome_funcao_executada>'
```

### Como criar um novo serviço

O *Application Server* possui três pastas que armazenam os principais arquivos que permitem a disponibilização de um novo serviço ao Atlas das Pastagens: 

1. **Controller:**: Os arquivos nesta pasta são responsáveis por processar implementar a lógica da tarefa passada pela requisição HTTP.

2. **Database**: Os arquivos nesta pasta são responsáveis por implementar os métodos com as *queries* a serem executadas no banco de dados. Devido a estrutura do `dataInjector.js`, mais de uma *query* poderá ser executada durante uma mesma requisição.

3.  **Routes**: Os arquivos desta pasta são os responsáveis por criar as URLs de acesso *(endpoint)* a um serviço, apontar qual o controlador deverá processar a lógica para a requisição e, caso necessário, injetar o acesso ao banco de dados.

Portanto, supondo que queremos criar um novo serviço para retornar o municpípio com a maior área de pastagem pertencente à tabela *pasture_col9_s100*:

    server/routes/example.js
``` js
module.exports = function (app) {

	var dataInjector = app.middleware.dataInjector
	var example = app.controllers.examplecontroller;

	app.get('/service/examplequery/largest', dataInjector, example.largest);

}
```

Após a criação do *endpoint* de acesso, basta criar o arquivo com as funções desta classe de *queries* (`examplequery.js`) e a função (`Query.largest`) que irá executar a *query* especificada. Vale ressaltar que deverá ser passado um parâmetro pela requisição, o **year**, que indicam de qual ano o usuário quer encontrar.

    server/database/queries/examplequery.js
``` js
module.exports = function (app) {

    var Query = {};
    
    Query.largest = function (params) {

        year = params['year'];
    
        return [{
                id: 'largest_id',
                sql: `SELECT estado, MAX(area_ha) AS max_area FROM pasture_col9_s100 WHERE year = ${year} and bioma='Cerrado' GROUP BY estado ORDER BY max_area DESC LIMIT 1;`
            }]
        };

        return Query;
    }
```

Após a construção do método e a *query*, basta criar o controlador `examplecontroller.js` para receber a requisição, realizar a chamada ao método para execução da *query*, coletar o resultado e enviar como resposta da requisição.

    server/controllers/examplecontroller.js
``` js
module.exports = function (app) {

    const config = app.config;
    
	var Controller = {}

	Controller.largest = function (request, response) {

		var queryResult = request.queryResult['largest_id']

		response.send(queryResult)

        response.end()

    }
    
    return Controller;
}
```

Por fim, este por se tratar de uma requisição HTTP do tipo `GET`, a mesma poderá ser acessada via navegador. Considerando que o server está executando em `localhost:3000` e o usuário deseja encontrar o município com maior area de pastagem de acordo com a Coleção 9 em 2020, a URL de acesso ficará da seguinte forma: 

``` url
http://localhost:3000/service/examplequery/largest?year=2020
```

Além de requisitar pelo navegador, o serviço também poderá ser requisitado pelo *Client* a fim de disponibilizar este dado na plataforma Atlas das Pastagens. Para tal, o mesmo poderá ser feito via biblioteca [HttpClient](https://angular.io/api/common/http/HttpClient) do Angular e assim obter o arquivo JSON com os dados processados. Considerando que a variável `http` foi devidadmente injetada no construtor da classe do Angular, o serviço `ExampleService` abaixo deverá realizar a requisição e retornar um *Observable* que poderá ser utilizado pela plataforma para obtenção dos dados:

    client/src/app/@core/services/example.service.ts
``` js
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { map } from "rxjs/operators";

export { HttpService };

@Injectable({
  providedIn: 'root'
})
class ExampleService {
    private apiURL = '/service/examplequery/largest';

    constructor(private httpClient: HttpClient) {}

    getData(year: int): Observable<any> {
        return this.httpClient.post<any>(this.apiURL, {
            headers: new HttpHeaders({
            'year': recaptcha,
            }),
        })
        .pipe(map(response => JSON.parse(response.data)));
    }
}
```
