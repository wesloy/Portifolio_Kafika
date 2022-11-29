# KAFKA
[![NPM](https://img.shields.io/npm/l/react)](https://github.com/wesloy/Portifolio_S.O.L.I.D/blob/main/license) 

## Sobre o projeto
Este projeto tem como intuito entender o Kafka utilizando C# 

## Montagem do Contêiner

- Pré-requisito: Docker instalado e rodando.
- Criar arquivo nomeado como: *docker-compose.yaml*
- Salvar as informações abaixo, no arquivo criado:

```yaml

version: "3.0"

services:

  zookeeper:
    image: confluentinc/cp-zookeeper:5.1.2
    restart: always
    environment:
      ZOOKEEPER_SERVER_ID: 1
      ZOOKEEPER_CLIENT_PORT: "2181"
    ports:
      - "2181:2181"
                  
  kafka1:
    image: confluentinc/cp-enterprise-kafka:5.1.2
    depends_on:
      - zookeeper
    ports:
    # Exposes 29092 for external connections to the broker
    # Use kafka1:9092 for connections internal on the docker network
    # See https://rmoff.net/2018/08/02/kafka-listeners-explained/ for details
      - "9092:9092"
    environment:
      KAFKA_ZOOKEEPER_CONNECT: "zookeeper:2181"
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka1:29092,PLAINTEXT_HOST://localhost:9092
      #KAFKA_LOG4J_ROOT_LOGLEVEL: INFO
      KAFKA_BROKER_ID: 1
      KAFKA_BROKER_RACK: "r1"
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      KAFKA_DELETE_TOPIC_ENABLE: "true"
      KAFKA_AUTO_CREATE_TOPICS_ENABLE: "true"
      KAFKA_JMX_PORT: 9991

```

- Com o terminal aberto (CMD) e na pasta que você salvou o arquivo acima, execute: *docker-compose -f docker-compose.yaml up*

**Vamos confirmar se o contêiner está okay?**  
É só executar o commando: *docker container ls*

**Deverá aparecer os 2 contêineres criados:**  
*confluentinc/cp-enterprise-kafka:5.1.2  
confluentinc/cp-zookeeper:5.1.2*


## Links Úteis / Referências
Ygor Polvere - Contêiner Kafka - https://medium.com/@ygorppolvere/kafka-com-docker-dedbbe9eff76  
Docker Configuration - https://docs.confluent.io/platform/current/installation/docker/config-reference.html  


