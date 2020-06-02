# jenkins-tutorial

Este repositório é baseado na documentação: [Build a Node.js and React app with npm](https://jenkins.io/doc/tutorials/build-a-node-js-and-react-app-with-npm/)

O repositório contém um aplicativo Node.js e React simples que gera
uma página com o conteúdo "Bem-vindo ao React" e com um teste para
verificar se o aplicativo é renderizado conforme esperado crianda nosso primeiro pipeline.

1. [Requisitos](#Requisitos)
2. [Início](#Início)
3. [Desbloqueio](#Desbloqueio)
4. [Pipeline](#Pipeline)

---

## Requisitos  <a name="Requisitos"></a>

Utilizaremos a arquitetura baseada em containers para instanciar a primeira versão do Jenkins e demonstrar a configuração de um pipeline utilizando o plugin Blue Ocean, este laboratório é baseado na documentação [Build a Node.js and React app with npm ](https://www.jenkins.io/doc/tutorials/build-a-node-js-and-react-app-with-npm/#setup-wizard);

1.1 Instalação do Docker CE: Para execução deste laboratório é necessário a instalação do serviço docker, existem versões para Windows, Linux e Mac, a documentação de apoio está disponível neste endereço: [Install Docker Engine](https://docs.docker.com/engine/install/);

1.2 Instalação do Docker Compose, embora não seja um pré-requisito a infraestrutura de containers construida na documentação foi automatizada para este laboratório utilizando [Overview of Docker Compose](https://docs.docker.com/compose/) este recurso automatiza algumas etapas da configuração com a inicialização dos dcontainers e configuração de rede e volumes, por isso para seguir o passo-a-passo abaixo sua instalação é necessária e [esta detalhada nesse documentação](https://docs.docker.com/compose/install/);

## Início <a name="Início"></a>

2.1 Como executarmeos algumas alterações durante o laboratório é necessário que você execute um [Fork deste repositório](https://help.github.com/pt/github/getting-started-with-github/fork-a-repo) para sua conta local no GitHub, utilize um repositório aberto para evitar a configurações de credenciais de acesso no CI.

2.2 Acesse o servidor disponibilizado para o laboratório e caso ainda não esteja disponível execute um clone deste repositório, em seguida inicie o stack do Jenkins a partir da raiz do projeto:

```sh
docker-compose up
```

## Desbloqueio <a name="Desbloqueio"></a>  #Liberando o jenkins para uso;

3.1 Como esta é a primeira vez que o serviço pe inicializado será criada uma credencial provisória para acesso administrativo, essa informação será publicada ao final do processo de inicialização dos containers conforme abaixo.

Obs.: Se estiver utilizando um segundo terminal ou iniciou o container em background será possível recuperar as credenciais utilizando o comando abaixo:

```sh
docker logs jenkins-tutorial
```

3.2 Depois de [desbloquear o Jenkins](https://www.jenkins.io/doc/tutorials/build-a-node-js-and-react-app-with-npm/#unlocking-jenkins), a página **Customize Jenkins** será exibida

3.3 Nesta página, clique em **"Install suggested plugins."**

> O assistente de instalação mostra a progressão na instalação dos plug-ins sugeridos. Esse processo pode demorar alguns minutos.

3.4 Em seguida o Jenkins solicitará que você crie seu primeiro usuário administrador, especifique os detalhes nos respectivos campos, anote a senha utilizada e clique em Salvar e concluir.

3.5 Quando a página Jenkins estiver pronta aparecer, clique em **"Start using Jenkins"**

---

## Pipeline <a name="Pipeline"></a> #Criando o primeiro pipeline

4. Crie seu projeto de pipeline no Jenkins, para isso na página **"Welcome to Jenkins!"** clique em criar **"Create New Job"**

4.1 No campo que surgir digite um nome para o seu novo projeto de Pipeline (por exemplo, scj-simple-js-react-app).

4.2 Em seguida logo abaixo escolha a opção **"Pipeline"**, depois clique em OK no final da página.

> (Opcional) Na página seguinte, especifique uma breve descrição para o seu Pipeline no campo Descrição (por exemplo, um Pipeline de entrada demonstrando como usar o Jenkins para criar um aplicativo Node.js na mba =} )

4.3 Na parte superior da página clique na Guia Pipeline para rolar até a sessão de configuração do nosso Pipeline;

4.4 No campo **"Definition"**, escolha a opção Script de pipeline no SCM. Esta opção instrui Jenkins a obter seu Pipeline do Source Control Management (SCM), que será seu repositório Git clonado localmente, **essa será nossa primeira ação prática rumo a integração contínua;**

4.5 No campo SCM, escolha Git.

4.6 No campo URL do Repositório, especifique o repositório clonado, por exemplo: https://github.com/fiapsecdevops/jenkins-tutorial.git

**Importante:** Tome o cuidado de utilizar o seu repositório para que tenha o acesso necessário para fazer alterações no futuro!

4.7 Clique em Salvar para salvar seu novo Pipeline. Agora você está pronto para começar a criar seu [Jenkinsfile](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/);

