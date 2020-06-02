# jenkins-tutorial

Este repositório é baseado na documentação: [Build a Node.js and React app with npm](https://jenkins.io/doc/tutorials/build-a-node-js-and-react-app-with-npm/)

O repositório contém um aplicativo Node.js e React simples que gera
uma página com o conteúdo "Bem-vindo ao React" e com um teste para
verificar se o aplicativo é renderizado conforme esperado crianda nosso primeiro pipeline.

O repositório contém um **Jenkinsfile** e um subdiretório com scripts em shell que são executados quando o Jenkins processa as etapas do Pipeline.

---

# Primeiros Passos:

Utilizaremos a arquitetura baseada em containers para instanciar a primeira versão do Jenkins e demonstrar a configuração de um pipeline utilizando o plugin Blue Ocean, este laboratório é baseado na documentação [Build a Node.js and React app with npm ](https://www.jenkins.io/doc/tutorials/build-a-node-js-and-react-app-with-npm/#setup-wizard);

## Requisitos

1. Instalação do Docker CE: Para execução deste laboratório é necessário a instalação do serviço docker, existem versões para Windows, Linux e Mac, a documentação de apoio está disponível neste endereço: [Install Docker Engine](https://docs.docker.com/engine/install/);

2. Instalação do Docker Compose, embora não seja um pré-requisito a infraestrutura de containers construida na documentação foi automatizada para este laboratório utilizando [Overview of Docker Compose](https://docs.docker.com/compose/) este recurso automatiza algumas etapas da configuração com a inicialização dos dcontainers e configuração de rede e volumes, por isso para seguir o passo-a-passo abaixo sua instalação é necessária e [esta detalhada nesse documentação](https://docs.docker.com/compose/install/);

## Iniciando o Laboratório

1. Acesse o servidor disponibilizado para o laboratório e caso ainda não esteja disponível execute um clone deste repositório, em seguida inicie o stack do Jenkins a partir da raiz do projeto:

```sh
docker-compose up
```

2. Como esta é a primeira vez que o serviço pe inicializado será criada uma credencial provisória para acesso administrativo, essa informação será publicada ao final do processo de inicialização dos containers conforme abaixo.

