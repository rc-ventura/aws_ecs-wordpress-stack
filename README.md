
# README: Implantação Contínua de Aplicação WordPress com MySQL no Minikube e no GKE

Este README fornece instruções para implantar continuamente uma aplicação WordPress com um banco de dados MySQL localmente usando o Minikube e na nuvem usando o Google Kubernetes Engine (GKE). Ele também inclui um roadmap, requisitos, comandos para autenticação na CLI do Google Cloud e informações sobre contribuições, tecnologias usadas, licença e colaboradores.

## Roadmap

1. **Implantação Local (Minikube)**
   - Instalar o Minikube e o Kubectl
   - Configurar um cluster Minikube local
   - Implantar as aplicações MySQL e WordPress
   - Configurar o WordPress

2. **Implantação na Nuvem (GKE)**
   - Criar uma conta na Google Cloud Platform (GCP)
   - Instalar o Google Cloud SDK
   - Configurar um cluster GKE
   - Implantar as aplicações MySQL e WordPress
   - Configurar o WordPress
   - Expor a aplicação para a internet

3. **Entrega Contínua com GitLab CI/CD**
   - Configurar o pipeline de CI/CD no GitLab
   - Automação de implantação contínua

## Requisitos

### Implantação Local (Minikube)

- **Minikube**: Você precisa ter o Minikube instalado localmente. Você pode encontrar as instruções de instalação [aqui](https://minikube.sigs.k8s.io/docs/start/).

### Implantação na Nuvem (GKE)

- **Conta da Google Cloud Platform (GCP)**: Você precisa de uma conta na GCP para usar o Google Kubernetes Engine. Cadastre-se [aqui](https://cloud.google.com/).

- **Google Cloud SDK**: Instale o Google Cloud SDK seguindo as instruções [aqui](https://cloud.google.com/sdk/docs/install).

### Entrega Contínua com GitLab CI/CD

- **GitLab**: Você precisa de uma conta no GitLab para configurar os pipelines de CI/CD. Cadastre-se [aqui](https://gitlab.com/).

## Implantação Local (Minikube)

1. Inicie o Minikube:
   ```shell
   minikube start
   ```

2. Implantar o MySQL e o WordPress:
   ```shell
   kubectl apply -f kubernetes/mysql-deployment.yaml
   kubectl apply -f kubernetes/wordpress-deployment.yaml
   ```

3. Configure o WordPress:
   - Acesse o serviço do WordPress executando:
     ```shell
     minikube service wordpress-service
     ```
     Isso abrirá o WordPress em seu navegador padrão. Siga as instruções na tela para configurar seu site WordPress.

## Implantação na Nuvem (GKE)

1. Crie um Cluster GKE:
   ```shell
   gcloud container clusters create meu-cluster-gke --num-nodes=3 --zone=us-central1-a
   ```

2. Implantar o MySQL e o WordPress:
   ```shell
   kubectl apply -f kubernetes/mysql-deployment.yaml
   kubectl apply -f kubernetes/wordpress-deployment.yaml
   ```

3. Configure o WordPress:
   - Acesse o serviço do WordPress:
     ```shell
     kubectl get svc wordpress-service
     ```
     Copie o endereço IP externo e acesse-o em seu navegador da web para configurar o WordPress.

4. Expor a aplicação para a internet (opcional):
   ```shell
   kubectl expose deployment wordpress-deployment --type=LoadBalancer --port=80 --target-port=80
   ```

## Autenticação na CLI do Google Cloud

Autentique-se com o Google Cloud usando os seguintes comandos:

1. Autorize sua conta:
   ```shell
   gcloud auth login
   ```

2. Defina o ID do seu projeto:
   ```shell
   gcloud config set project SEU_ID_DE_PROJETO
   ```

3. Defina a zona de computação padrão:
   ```shell
   gcloud config set compute/zone SUA_ZONA
   ```

4. Configure o Docker para usar o GCR (Google Container Registry):
   ```shell
   gcloud auth configure-docker
   ```

Claro, aqui estão os passos para criar um cluster padrão no Google Kubernetes Engine (GKE) e implantar cargas de trabalho nele:

## Criar um Cluster GKE Padrão

1. Crie um cluster GKE com um único comando (substitua `<SEU_CLUSTER>` e `<SUA_ZONA>` pelos valores desejados):

   ```shell
   gcloud container clusters create <SEU_CLUSTER> --zone <SUA_ZONA>
   ```

   Por exemplo:

   ```shell
   gcloud container clusters create meu-cluster-gke --zone us-central1-a
   ```

2. Aguarde até que o cluster seja provisionado. Isso pode levar alguns minutos.

3. Configure o contexto do cluster como o contexto padrão para o Kubernetes:

   ```shell
   gcloud container clusters get-credentials <SEU_CLUSTER> --zone <SUA_ZONA>
   ```

   Por exemplo:

   ```shell
   gcloud container clusters get-credentials meu-cluster-gke --zone us-central1-a
   ```

   Agora você está pronto para implantar cargas de trabalho no cluster GKE.

## Implantar Cargas de Trabalho no Cluster GKE

1. Para implantar um arquivo de manifesto Kubernetes, use o comando `kubectl apply -f` seguido do caminho para o arquivo YAML que descreve sua carga de trabalho. Por exemplo:

   ```shell
   kubectl apply -f seu-arquivo-de-manifesto.yaml
   ```

2. Para implantar um contêiner Docker em um pod, você pode usar o comando `kubectl run`. Substitua `<NOME_DO_POD>`, `<IMAGEM_DOCKER>` e outras opções pelo seu caso específico. Por exemplo:

   ```shell
   kubectl run <NOME_DO_POD> --image=<IMAGEM_DOCKER> --port=<PORTA>
   ```

3. Para verificar o status das implantações e pods no cluster, você pode usar os seguintes comandos:

   - Listar implantações:

     ```shell
     kubectl get deployments
     ```

   - Listar pods:

     ```shell
     kubectl get pods
     ```

   - Obter informações detalhadas sobre um pod específico:

     ```shell
     kubectl describe pod <NOME_DO_POD>
     ```

Lembre-se de personalizar os comandos e arquivos de manifesto de acordo com suas necessidades específicas. Com esses comandos, você poderá criar um cluster GKE padrão e implantar cargas de trabalho nele de maneira eficaz.

## Entrega Contínua com GitLab CI/CD

### Configurar o Pipeline de CI/CD no GitLab

1. No seu repositório do GitLab, crie um arquivo chamado `.gitlab-ci.yml` e configure seu pipeline de CI/CD. Aqui está um exemplo simples:

   ```yaml
   stages:
     - build
     - deploy

   build:
     stage: build
     script:
       - echo "Construindo e testando o código aqui..."

   deploy:
     stage: deploy
     script:
       - echo "Implantando no ambiente de produção aqui..."
     only:
       - master
   ```

   Este é um exemplo básico. Você pode personalizar seu pipeline de acordo com suas necessidades específicas.

### Automação de Implantação Contínua

1. Configure seus scripts de implantação no estágio de `deploy` no arquivo `.gitlab-ci.yml`. Por exemplo, você pode usar os mesmos comandos de implantação que usou localmente no Minikube ou no GKE.

2. Quando você faz push para a branch `master`, o GitLab CI/CD automaticamente executa o pipeline de CI/CD e implanta as atualizações no ambiente de produção.

3. Acompanhe o status e os logs do pipeline no GitLab para garantir que tudo esteja funcionando conforme o esperado.

Agora, você tem um pipeline de CI/CD configurado no GitLab para implantar continuamente sua aplicação WordPress em um ambiente de produção. Personalize-o de acordo com as necessidades do seu projeto.

## Contribuições

Nós adoramos contribuições da comunidade! Se você quiser contribuir para este projeto, siga estas etapas:

1. Faça um fork do repositório.
2. Crie uma branch para sua contribuição: `git checkout -b sua-contribuicao`.
3. Faça suas alterações e adições.
4. Faça um commit com uma mensagem descritiva: `git commit -m "Adicionei funcionalidade X"`.
5. Envie suas alterações para o seu fork: `git push origin sua-contribuicao`.
6. Crie um pull request para que possamos revisar suas alterações.

## Tecnologias Usadas

Este projeto utiliza as seguintes tecnologias:

- [WordPress](https://wordpress.org/)
- [MySQL](https://www.mysql.com/)
- [Minikube](https://minikube.sigs.k8s.io/)
- [Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine)
- [Google Cloud Platform (GCP)](https://cloud.google.com/)
- [GitLab CI/CD](https://docs.gitlab.com/ee/ci/)

## Licença e Direitos de Uso

Este projeto é licenciado sob a [Licença MIT](LICENSE). Você é livre para usar, modificar e distribuir este software de acordo com os termos da licença.

## Colaboradores

Agradecemos aos seguintes colaboradores por suas contribuições para este projeto:



- [Nome do Colaborador 1](link-para-o-perfil-do-colaborador-1)
- [Nome do Colaborador 2](link-para-o-perfil-do-colaborador-2)

Se você contribuiu para este projeto e deseja ser adicionado à lista de colaboradores, sinta-se à vontade para abrir um pull request ou entrar em contato conosco.

