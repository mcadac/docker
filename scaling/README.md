
# Docker scaling with docker compose

## Build nginx-scale image
1. docker image build -t nginx-scale:latest ./

## Run container
1. docker-compose up -d
2. docker ps
3. docker-compose scale nginx=3
4. docker-compose up -d --scale nginx=4
