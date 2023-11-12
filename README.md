# Kafka + go simple service

## Run
Donwload Bitnami’s Kafka Docker image
```bash
curl -sSL https://raw.githubusercontent.com/bitnami/containers/main/bitnami/kafka/docker-compose.yml > docker-compose.yml
```
 
Before starting the Kafka broker, there’s a slight modification required in the docker-compose.yml file. Find the following string:
```bash
KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://:9092
```
And replace it with:
```bash
KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092
```
Run:
```bash
docker-compose up -d
```

Run the producer
```bash
go run cmd/producer/producer.go
```
Start the consumer
```bash
go run cmd/consumer/consumer.go
```

## How to send message
User 1 (Emma) receives a notification from User 2 (Bruno):
```bash
curl -X POST http://localhost:8080/send \
-d "fromID=2&toID=1&message=Bruno started following you."
```
User 2 (Bruno) receives a notification from User 1 (Emma):
```bash
curl -X POST http://localhost:8080/send \
-d "fromID=1&toID=2&message=Emma mentioned you in a comment: 'Great seeing you yesterday, @Bruno!'"
```

User 1 (Emma) receives a notification from User 4 (Lena):
```bash
curl -X POST http://localhost:8080/send \
-d "fromID=4&toID=1&message=Lena liked your post: 'My weekend getaway!'"
```

## How to retrieving notifications
Retrieving notifications for User 1 (Emma):
```bash 
curl http://localhost:8081/notifications/1
```