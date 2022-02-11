# kafka-connector-example


## Descrição

Exemplo do uso de conectores para ingestão de dados de um arquivo CSV (Source) para tópicos do Kafka e posterior inserção (Sink) destes dados em um banco dados relacional (PostgreSQL).

O projeto foi criado para estudo de conectores do Kafka.

## Conectores utilizados

Para leitura do arquivo (neste caso um arquivo .csv), foi utilizado o conector [kafka-connect-file-pulse](https://github.com/streamthoughts/kafka-connect-file-pulse)

Para consumo das mensagens publicadas no tópico e inserção destes dados no banco de dados relacional, foi utilizado o conector [JDBC Connector](https://www.confluent.io/hub/confluentinc/kafka-connect-jdbc)


## Arquivo utilizado

Como fonte de dados foi utilizado um arquivo .csv com dados de avaliação de filmes, disponível no [Kaggle](https://www.kaggle.com/rounakbanik/the-movies-dataset?select=ratings.csv)


## Execução do projeto

Para executar o projeto, é necessário que o `docker` e o `docker-compose` estejam instalados.

Faça o download deste repositório, e dentro do diretório criado execute o comando:

```
docker-compose up -d
```

Para configuração do `kafka-connect-file-pulse`, execute o comando:

```
curl -sX PUT http://localhost:8083/connectors/conector-de-teste/config \
-d @config/connector-config.json \
--header "Content-Type: application/json"
```

A configuração do conector `JDBC Connector` deve ser realizada pelo control-center (http://localhost:9021/). Clique para adicionar um novo conector e faça upload do [arquivo de configuração](./config/jdbc.properties)

Mova o arquivo de [input](./input/movies.csv) para a pasta que foi criada pelo mapeamento de volume definido no `docker-compose.yml`

O conector `file-pulse` irá processar o arquivo, publicar cada uma das linhas como mensagens no tópico do Kafka e o conector `JDBC Connector`, por sua vez, irá consumir estas mensagens e inserir no banco de dados.