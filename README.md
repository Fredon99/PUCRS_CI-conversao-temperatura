# Requisitos
- [Docker](https://www.docker.com/)
- [NodeJS](https://nodejs.org/)

# conversao-temperatura

* Instalação das dependências
```
Entrar no diretório src/
└──> npm install
```

* Rodar aplicação local
```
Através do diretório src/
└──> node server.js
Servidor rodando na porta 8080
```

* Construindo Imagem docker através do Dockerfile
```
Dentro de src/ executar o seguinte comando "docker build -t <image-name>:<tag> <path-to-dockerfile>", no meu caso em específico ficou:
└──> docker build -t fredon99/conversao-temperatura:v1 .
```

* Validando criação da imagem
```
└──> docker image ls
REPOSITORY                       TAG       IMAGE ID       CREATED         SIZE
fredon99/conversao-temperatura   v1        d9575faef25a   8 seconds ago   939MB
```

* Testar a aplicação e imagem do Docker
```
Executar o comando docker container run -d -p 8080:8080 <image-name>:<tag>, no meu caso ficou:
└──> docker container run -d -p 8080:8080 fredon99/conversao-temperatura:v1
b79616ed399b572d31eeaa5c764c3eaacf84833325725cc4cb70011f79a7d939 
```

* Salvar a imagem docker no DockerHub através do comando "docker push <image-name>:<tag>", que ficou:
```
└──> docker push fredon99/conversao-temperatura:v1
The push refers to repository [docker.io/fredon99/conversao-temperatura]
4ec46bebe800: Preparing
fd435a6000a0: Preparing
fd435a6000a0: Pushed
598bc5ae93c5: Pushed
.
.
. 
v1: digest: sha256:d3883f770e0daeef418b9b5becb00f56f6acfc6e40e43559e3c2234a4cafa835 size: 3050
```

* Este respositório possui um workflow para Integração Contínua, que pode ser visualizado através do link. [:link:](https://github.com/Fredon99/PUCRS_CI-conversao-temperatura/blob/master/.github/WORKFLOW-README.md)

* Docker Hub

[DockerHub](https://hub.docker.com/repository/docker/orbite82/conversao-temperatura-2)