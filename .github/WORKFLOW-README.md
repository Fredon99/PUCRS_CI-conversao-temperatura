# Continuous Integration Workflow

Este repositório possui um workflow de Integração Contínua (CI) utilizando GitHub Actions para automatizar testes e a construção de uma imagem Docker. O processo é acionado sempre que houver um push na branch `master`.

## Fluxo do Workflow

### 1. Configuração
O workflow é definido no arquivo YAML e nomeado como `CI`.

```yaml
name: CI
```

### 2. Gatilhos
O workflow será acionado nos seguintes eventos:
- `push` para a branch `master`

```yaml
on: 
    push:
        branches: [ master ]
    workflow_dispatch:
```

### 3. Jobs e Execução
O job principal `CI` será executado em uma máquina `ubuntu-latest` e seguirá os seguintes passos:

#### Passo 1: Checkout do Repositório
Baixa o código-fonte do repositório para a máquina runner.
```yaml
- uses: actions/checkout@v2
```

#### Passo 2: Configuração do Node.js
Instala a versão 16.13.2 do Node.js.
```yaml
- name: Setup do NodeJS
  uses: actions/setup-node@v3.0.0
  with:
    node-version: 16.13.2
```

#### Passo 3: Instalação do Mocha e Dependências
Navega para a pasta `src`, instala o Mocha globalmente e instala as dependências do projeto.
```yaml
- name: Instalacao do Mocha e dos pacotes para teste
  run: |
      cd src;
      npm install -g mocha;
      npm install
```

#### Passo 4: Execução dos Testes
Executa os testes localizados no arquivo `src/test/convert.js` utilizando Mocha.
```yaml
- name: Execucao do teste
  run: mocha src/test/convert.js
```

#### Passo 5: Login no DockerHub
Realiza o login no DockerHub utilizando as credenciais armazenadas nos secrets do repositório.
```yaml
- name: Login no DockerHub
  uses: docker/login-action@v3.3.0
  with:
      username: ${{secrets.DOCKERHUB_USERNAME}}
      password: ${{secrets.DOCKERHUB_PASSWORD}}
```

#### Passo 6: Construção e Publicação da Imagem Docker
A imagem Docker é construída a partir do `Dockerfile` na pasta `src`, e então enviada para o DockerHub.
```yaml
- name: Construcao da imagem docker
  uses: docker/build-push-action@v6.10.0
  with:
      context: ./src
      file: ./src/Dockerfile
      push: true
      tags: |
          fredon99/conversao-temperatura:latest
```

## Requisitos
Para que este workflow funcione corretamente, é necessário:
- Configurar os secrets `DOCKERHUB_USERNAME` e `DOCKERHUB_PASSWORD` no repositório GitHub.
- Ter um repositório configurado com Node.js e Mocha.
- O `Dockerfile` precisa estar localizado na pasta `src` e corretamente configurado.

## Como Executar Manualmente
Caso deseje executar o workflow manualmente:
1. Acesse a aba `Actions` no GitHub.
2. Selecione o workflow `CI`.
3. Clique em `Run workflow` e confirme.

## Resultado Esperado
- Se os testes passarem, a imagem Docker será construída e enviada para o DockerHub.
- Caso algum teste falhe, o processo será interrompido e indicado no GitHub Actions.
