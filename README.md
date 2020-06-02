# jenkins-tutorial

Este repositório é baseado na documentação: [Build a Node.js and React app with npm](https://jenkins.io/doc/tutorials/build-a-node-js-and-react-app-with-npm/)

O repositório contém um aplicativo Node.js e React simples que gera
uma página com o conteúdo "Bem-vindo ao React" e com um teste para
verificar se o aplicativo é renderizado conforme esperado crianda nosso primeiro pipeline.

1. [Requisitos](#Requisitos)
2. [Início](#Início)
3. [Desbloqueio](#Desbloqueio)
4. [Pipeline](#Pipeline)
5. [Sessões](#Sessões)
6. [Run!](#Run!)

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

4.1 Crie seu projeto de pipeline no Jenkins, para isso na página **"Welcome to Jenkins!"** clique em criar **"Create New Job"**

4.2 No campo que surgir digite um nome para o seu novo projeto de Pipeline (por exemplo, scj-simple-js-react-app).

4.3 Em seguida logo abaixo escolha a opção **"Pipeline"**, depois clique em OK no final da página.

> (Opcional) Na página seguinte, especifique uma breve descrição para o seu Pipeline no campo Descrição (por exemplo, um Pipeline de entrada demonstrando como usar o Jenkins para criar um aplicativo Node.js na mba =} )

4.4 Na parte superior da página clique na Guia Pipeline para rolar até a sessão de configuração do nosso Pipeline;

4.5 No campo **"Definition"**, escolha a opção Script de pipeline no SCM. Esta opção instrui Jenkins a obter seu Pipeline do Source Control Management (SCM), que será seu repositório Git clonado localmente, **essa será nossa primeira ação prática rumo a integração contínua;**

4.6 No campo SCM, escolha Git.

4.7 No campo URL do Repositório, especifique o repositório clonado, por exemplo: https://github.com/fiapsecdevops/jenkins-tutorial.git

**Importante:** Tome o cuidado de utilizar o seu repositório para que tenha o acesso necessário para fazer alterações no futuro!

4.8 Clique em Salvar para salvar seu novo Pipeline. Agora você está pronto para começar a criar seu [Jenkinsfile](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/);

**Vantagens no uso de um arquivo declarativo como o Jenkinsfile**

* Utilizando este método é possível estabelecer estratégias de Code Review para o próprio Pipeline de Integração Contínua;
* Também é possível auditar as etapas e literalmente "rastrear" o fluxo percorrido pelo produto sendo entregue via Pipeline; 
* Também conseguiremos um "Single source of truth" para o Pipeline, ou seja, ele poderá ser visualizado e editado por vários membros do projeto.


No Jenkins estes pipelines podem ser escritos utilizando [uma linguagem declarativa](https://www.jenkins.io/doc/book/pipeline/syntax/) ou a linguagem de programação [Groovy](http://groovy-lang.org/syntax.html);

---

## Exemplo <a name="Pipeline"></a> #Criando o primeiro pipeline

O conteúdo base necessário para o nosso primeiro pipeline está na raiz do repositório no arquivo JenkinsFile conforme abaixo:

```sh
pipeline {
    agent {
        docker {
            image 'node:12-alpine' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
    }
}
```

Este fluxo possui um único **stage** chamado Build, e um único agente chamado **docker** ambos os conceitos explicados abaixo:

---

## Sessões <a name="Sessões"></a> #A composição bruta de um pipeline em Jenkins

5.1 Um pipeline declarativo em Jenkins será sempre dividido em sessões, cada sessão possui uma função dentro do fluxo do pipeline.

5.2 A sessão **"agent"** especifica onde o Pipeline, ou um estágio específico dele será executado, essa seção deve ser obrigatoriamente declarada ao menos uma vez no nível superior de dentro do bloco de pipeline, também é possível declarar em niveis inferiores alternando entre ambientes mas este uso é opicional.

> Neste exemplo para a sessão agent utilizamos o tipo Docker o que provisionará um container que será o hospedeiro do ambiente responsável por executar a etapa seguinte, essa tática tem sido muito utilizada por desenvolvedores a fim de isolar dependências dentro do Pipeline em relação ao servidor hospedando a aplicação que será construída.

5.3 A sessão **"stage"** contém o ponto mais importante deste estudo, o core do pipeline, uma sequência de uma ou mais diretivas com as funções que constroem os estágios é onde a maior parte do "trabalho" descrito por um Pipeline será localizado a partir de ações como compilar, testar e implementar, etc.

---

## Run! <a name="Run!"></a> #Execute o primeiro pipeline do laboratório

6.1 Após verificarmos a esturtura por trás do pipeline faremos a primeira execução utilizando o plugin blue ocean, para isso volte para Jenkins novamente, faça login se necessário e clique em **"Open Blue Ocean"** no painél à esquerda para acessar a interface do plugin.

6.2 Você provavelmente verá uma mensagem **"This job has not been run"**, clique em **"Run"** e, em seguida, clique rapidamente no link **"OPEN"** que aparece brevemente no canto inferior direito para ver Jenkins construindo seu projeto. 

6.3 Se não foi possível clicar no link, clique na linha que aparece interface principal do Blue Ocean para acessar esse recurso.

> O Blue Ocean é um plugin extremamente popular que trás como proposta repensar a experiência do usuário no CI. Ele basicamente projeta o Pipeline e cria um modelo visual para os estágios, o que reduz a desordem e aumenta a clareza para todos os membros da equipe, vale apena uma lida na documentação do projeto para os interessados em maiores detalhes;

[jenkins.io: What is Blue Ocean?](https://www.jenkins.io/doc/book/blueocean/)





 
