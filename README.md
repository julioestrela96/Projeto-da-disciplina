Virtualização 2023.2 - Projeto da disciplina
Objetivo

O objetivo do projeto é configurar uma aplicação web usando o Strapi e o PostgreSQL, implantando-a em um cluster Kubernetes.
Serviço
O serviço a ser implementado será uma aplicação web simples usando o Strapi como framework. O Strapi é uma plataforma de conteúdo headless que permite criar APIs e aplicações web baseadas em conteúdo. O PostgreSQL será usado como banco de dados para armazenar os dados da aplicação.

Para a aplicação, utilizaremos a Food Advisor https://github.com/strapi/foodadvisor.git, uma aplicação de demonstração implementada no Stripe. Para tal, a ideia é fazer o clone desse repositório, a construção da imagem docker, a criação do Helm chart e em seguida a implantação em um cluster Kubernetes.
Recursos do Kubernetes
Os recursos do Kubernetes necessários para o projeto são:

Para o Strapi:
Dois pods: um para api e outro para o client 
Dois serviços: um para api e outro para o client 
Um secret para guardar as credenciais de conexão da api com o banco
Dois deployment: um para api e outro para o client 
Dois ingress: um para api e outro para o client 
Para o PostgreSQL:
Utilize o chart pronto do PostgreSQL disponível no repositório da Bitnami: https://github.com/bitnami/charts/tree/main/bitnami/postgresql

Criação da imagem Docker

Será necessária a criação de duas imagens Docker, uma para a API e outra para o client. Siga este tutorial para criar os Dockerfile referentes à cada imagem e fazer a construção das imagens da api e do client: https://strapi.io/blog/deploying-and-scaling-the-official-strapi-demo-app-food-advisor-with-kubernetes-and-docker



Com a imagem Docker criada, crie o helm chart da aplicação usando o seguinte comando:

helm create strapi-foodadvisor

O chart Helm será configurado para usar a imagem Docker criada anteriormente. Também será configurada a dependência do chart para o Postgresql. Lembre-se de checar as variáveis de ambientes necessárias para fazer a conexão da api com o postgres. Pode-se encontrar a definição dessa variáveis aqui: https://github.com/strapi/foodadvisor/blob/master/api/config/database.js 

Implantação do serviço

O serviço será implantado usando o seguinte comando:

helm install strapi-foodadvisor ./strapi-foodadvisor

Teste do serviço

O serviço poderá ser testado usando o seguinte comando:

kubectl port-forward svc/ingress-nginx-controller 8889:80 -n ingress-nginx

Após, basta acessar o navegador na porta 8889 e referenciar o path que foi configurado no ingress para api e para o client. Ele deve deverá exibir as aplicações web.

Aqui estão alguns detalhes adicionais que você pode considerar ao especificar seu projeto:

Qual versão do Strapi e do PostgreSQL você deseja usar?
Como você deseja testar o serviço? Você pode usar um cluster Kubernetes local (minikube) ou um ambiente de teste.


