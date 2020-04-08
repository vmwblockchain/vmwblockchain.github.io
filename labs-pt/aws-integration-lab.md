---
layout: single
title: "Integração com AWS"
categories: labs
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
categories: labs
---

# Introdução

Uma das razões mais atrativas para se adotar o VMware Cloud na AWS é integrar seus sistemas existentes que estão em seu ambiente de nuvem VMware, com plataformas de aplicações que residem em seu ambiente AWS em VPCs. A integração que a VMware e a AWS criaram permite que esses serviços se comuniquem gratuitamente (se dentro da mesma AZ - Availability Zone ou Zona de Disponibilidade) para serviços como instâncias do EC2, que se conectam a sub-redes dentro de um VPC nativo da AWS ou com serviços de plataforma que têm a capacidade de conectar-se a um VPC Endpoint, como o S3.

## Entendendo a integração do VMware Cloud on AWS com a AWS

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-1.jpg)

Como o diagrama acima ilustra, os workloads VMware não apenas ficam ao lado dos serviços da AWS, mas são totalmente integrados a esses serviços. Isso introduz uma nova maneira de pensar sobre como projetar e aproveitar os serviços da AWS com o SDDC da VMware. Algumas integrações que nossos clientes estão usando incluem:

* Front-end em VMware e back-end do RDS
* Back-end em VMware e front end do EC2
* Balanceador de Carga da AWS (ELBv2) com front-end do VMware (apontando para IPs privados)
* Lambda, Simple Queueing Service (SQS), Simple Notification Service (SNS), S3, Route53, and Cognito
* AWS Lex e Alexa com as APIs do VMware Cloud

Estas são apenas algumas das integrações que vimos. Existem muitos serviços diferentes que podem ser integrados ao seu ambiente.
Neste exercício, exploraremos as integrações com o AWS Simple Storage Service (S3) e o AWS Relational Database Service (RDS).

**Nota: Existe um requisito neste laboratório e é necessário ter concluído os passos no [Trabalhando com o seu SDDC] (https://vmc-field-team.github.io/labs-pt/working-with-sddc-lab/) sobre criação de Content Library, criação de Redes e criação de regras de firewall.**

### Como essas integrações são possíveis?

Além de ficar na infraestrutura da AWS, há uma Interface de Rede Elástica (ENI) conectando o VMware Cloud na AWS e a Virtual Private Cloud (VPC) do cliente, fornecendo uma conexão de baixa latência e alta largura de banda entre o VPC e o SDDC. É onde o tráfego flui entre as duas tecnologias (VMware e AWS). Não há cobranças de EGRESS na ENI dentro da mesma Zona de disponibilidade e há firewalls nas duas extremidades dessa conexão para fins de segurança.

### Como o tráfego é protegido na ENI?

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-2.jpg)

Do lado do VMware (veja a imagem abaixo), o ENI entra no SDDC no Compute Gateway (NSX Edge). Isso significa que, neste lado da tecnologia, permitimos e proibimos o tráfego das regras ENI com o Firewall do NSX. Por padrão, nenhum tráfego ENI pode entrar no SDDC. Pense nisso como um portão de segurança que bloqueia o tráfego de e para o os serviços da AWS na ENI até que as regras sejam modificadas.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-3.jpg)

No lado dos serviços da AWS (veja a imagem abaixo), os grupos de segurança são utilizados. Para aqueles que não estão familiarizados com os grupos de segurança, eles atuam como um firewall virtual para diferentes serviços (VPCs, bancos de dados, instâncias EC2, etc.). Isso deve ser configurado para negar tráfego de e para o VMware SDDC, a menos que seja configurado de outra forma.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-4.jpg)

Neste exercício, tudo foi configurado no lado da AWS para você. No entanto, você aprenderá como abrir o tráfego da AWS para conectividade com o VMware Cloud no AWS SDDC.

### Regras de Firewall do Compute Gateway para Serviços Nativos da AWS

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-5.jpg)

1\. No portal VMware Cloud on AWS, clique na guia **Networking & Security**

2\. Clique em **Groups**

3\. Clique **ADD GROUP**

#### Nomeie o Grupo de Workloads

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-6.jpg)

1\. Escreva no nome **PhotoAppVM**

2\. Deixe **Virtual Machine** selecionado para o Member Type

3\. Clique **Set VMs** em Members

#### Selecionando as Máquinas Virtuais para o Grupo de Workloads

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-7.jpg)

1\. Clique e selecione **Webserver01**

2\. Clique **SAVE**

#### Salvando o Grupo de Workloads

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-8.jpg)

1\. Clique **SAVE**

### Regras de Firewall

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-9.jpg)

1\. Clique em **Networking & Security** no Portal do VMware Cloud on AWS

2\. Clique **Gateway Firewall**

3\. Clique e selecione **Compute Gateway**

4\. Clique **ADD NEW RULE**

#### Adicionando uma nova regra de firewall - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-10.jpg)

1\. Escreva o nome da regra **AWS Inbound**

2\. Clique em **Set Source**

#### Selecionando a Origem - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-11.jpg)

1\. Clique e selecione **Connected VPC Prefifixes**

2\. Clique **SAVE**

#### Selecionando o Destino - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-12.jpg)

1\. Clique em **Set Destination**

#### Selecionando o Destino - AWS Inbound (Continuação)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-13.jpg)

1\. Clique e selecione **PhotoAppVM**

2\. Clique **SAVE**

#### Selecionando o Serviço - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-14.jpg)

1\. Clique em **Set Service**

#### Selecionando o Serviço - AWS Inbound (Continuação)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-15.jpg)

1\. Clique e selecione **Any**

2\. Clique **SAVE**

#### Publicando - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-16.jpg)

1\. Clique em **PUBLISH**

**Nota:** Certifique-se de selecionar **All Uplinks** no campo **Applied To**.

#### Adicionando uma Nova Regra de Firewall - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-17.jpg)

1\. Clique **ADD NEW RULE**

2\. Coloque o nome **AWS Outbound**

3\. Clique em **Set Source**

#### Selecionando a Origem - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-18.jpg)

1\. Clique e selecione **PhotoAppVM**

2\. Clique **SAVE**

#### Selecionando o Destino - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-19.jpg)

1\. Clique em **Set Destination**

#### Selecionando o Destino - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-20.jpg)

1\. Clique e selecione **Connected VPC Prefifixes**

2\. Clique **SAVE**

#### Selecionando o Serviço - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-21.jpg)

1\. CLique em **Set Service**

#### Selecionando o Serviço - AWS Outbound (Continuação)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-22.jpg)

1\. Em **Select Services** escreva **3306**

2\. Selecione **MySQL**

3\. Clique **SAVE**

#### Publicando - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-23.jpg)

1\. Clique **PUBLISH**

**Nota:** Certifique-se de selecionar **All Uplinks** no campo **Applied To**.

### Adicionando Nova Regra - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-24.jpg)

1\. Clique em **ADD NEW RULE**

#### Adicionando Nova Regra - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-25.jpg)

1\. Digite **Public In** em Name

2\. Clique em **Set Source**

#### Selecionando a Origem - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-26.jpg)

1\. Clique e selecione **Any**

2\. Clique **SAVE**

#### Selecionando o Destino - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-27.jpg)

1\. Clique em **Set Destination**

#### Selecionando o Destino - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-28.jpg)

1\. Clique e Selecinoe **PhotoAppVM**

2\. Clique **SAVE**

#### Selecionando o Serviço - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-29.jpg)

1\. Clique **Set Service**

#### Selecionando o Serviço - Public In (Continuação)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-30.jpg)

1\. Escreva **HTTP 80** em **Select Services**

2\. Clique e selecione **HTTP**

3\. Clique **SAVE**

#### Publicando - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-31.jpg)

1. Clique **PUBLISH**

**Nota:** Certifique-se de selecionar **All Uplinks** no campo **Applied To**.

## Integrando com AWS Relational Database Service (RDS)

O Amazon RDS facilita a configuração, operação e dimensionamento de um banco de dados relacional na nuvem. Ele fornece capacidade redimensionável e econômica ao automatizar tarefas administrativas demoradas, como provisionamento de hardware, configuração de banco de dados, correções e backups. Ele libera você para se concentrar em seus aplicativos para que você possa oferecer a eles o desempenho rápido, alta disponibilidade, segurança e compatibilidade de que precisam.

Neste exercício, você poderá integrar uma máquina virtual no VMware Cloud on AWS para trabalhar em conjunto com um banco de dados relacional em execução no Amazon Web Services (AWS) que foi configurado anteriormente para você.

### Anote o IP do Webserver01

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS1.jpg)

Você estará usando a VM criada no módulo anterior para concluir este exercício.

1\. Na interface do vCenter para VMware Cloud na AWS, encontre o **Webserver01** configurada e verifique se foi atribuído um endereço IP, conforme mostrado no gráfico.

### Anexando um IP público

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS2.jpg)

1\. Volte para o seu portal VMware Cloud on AWS e clique na guia **Networking & Security** para solicitar um endereço IP público

2\. Clique **Public IPs**

3\. Clique em **REQUEST NEW IP**

4\. Em notas, escreva **PhotoAppIP**

5\. Clique **SAVE**

### Anote o IP Publico

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS3.jpg)

Tome nota do seu IP público recém-criado.

### Criando uma regra de NAT

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS4.jpg)

1\. Clique em **NAT** i

2\. Clique **ADD NAT RULE**

3\. Escreva **PhotoApp NAT** em Name

4\. Certifique-se de que o IP público que você solicitou na etapa anterior aparece em IP público

5\. Deixe **All Traffic** (no change)

6\. Digite o endereço IP do seu **Webserver01** que você anotou no início deste exercício

7\. Clique **SAVE**

### Integrando com o AWS Relational Database Service (RDS)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS24.jpg)

No seu browser, abra uma nova aba: https://vmcworkshop.signin.aws.amazon.com/console

1\. ID da conta ou alias - Consulte as informações fornecido a você para obter informações sobre a ID da conta.

2\. IAM user name - **Student#** (where # is the number assigned to you)

3\. Password - **VMCworkshop1211**

4\. Clique **Sign In**

Por favor, note que você pode obter um dos 2 sinais nas telas acima. Se você receber o da direita, insira o ID da conta e clique em **Next**

### Informação do RDS

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS6.jpg)

1\. Agora você está conectado ao console da AWS. Verifique se a região selecionada é **Oregon**

2\. Clique em **RDS** (Talvez você precise expandir em **All services**)

### Instância RDS

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS07.jpg)

1\. No lado esquerdo clique em **Databases**

2\. Clique na instância do RDS que corresponde ao número designado a você

### Navegando nos Grupos de Segurança

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS08.jpg)

1\. Role para baixo até a área **Details** e, em **Connectivity & Security**, observe que a instância do RDS não está acessível publicamente, o que significa que essa instância só pode ser acessada de dentro da AWS

2\. Clique no link em azul **Security Groups**

### Grupos de Segurança

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS9.jpg)

1\. Selecione o Grupo de Segurança do RDS **Student##-RDS-Inbound** correspondente a você (verifique com atenção!)

2\.Depois de destacar o grupo de segurança apropriado, clique na guia **Inbound** abaixo

**Observação: O VMware Cloud on AWS estabelece o roteamento no grupo de segurança do VPC padrão, somente o RDS pode aproveitar isso ou utilizar o seu próprio**

### Tráfego de Saída

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS10.jpg)

1\. Clique em **Outbound**

2\.Você pode ver todo o tráfego (internal to AWS) permitido, isso inclui seu VMware Cloud em redes lógicas do SDDC.

### Elastic Network Interface (ENI)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS11.jpg)

O AWS Relational Database Service (RDS) também cria sua própria Elastic Network Interface (ENI) para acesso separado do ENI criado pelo VMware Cloud na AWS.

1\. Clique em **Services** para voltar a Console Inicial

2\. Clique em **EC2**

### ENI (Continuação)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS12.jpg)

1\. No EC2 Dashboard clique em **Network Interfaces**

2\. Todos os ambientes do aluno pertencem à mesma conta da AWS, portanto, centenas de ENIs podem existir. Para minimizar o tipo de visualização, escreva **RDS** na área de pesquisa e pressione Enter para adicionar um filtro

3\. Destaque seu grupo de segurança **Student ## - RDS-Inbound** correspondente ao seu número de aluno com base na segunda octeto do bloco CIDR na última coluna.

    Neste exemplo, o bloco CIDR é 172.6.8.187, isso corresponderia ao aluno **6**

4\. Anote o **Primary private IPv4 IP** para a próxima etapa

### Photo App

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS13.jpg)

1\. No seu smartphone (tablet ou computador pessoal), abra um navegador e digite o endereço IP público que você solicitou no portal VMware Cloud on AWS na barra de endereço do navegador, seguido de /Lychee (diferencia maiúsculas de minúsculas), por exemplo: 1.2.3.4/Lychee

2\. Digite as informações de conexão do banco de dados abaixo (__case sensitive__), usando o endereço IP que você anotou na etapa anterior do RDS ENI:

    Database Host: x.x.x.x:3306
    Database Username: student# (aonde # é o número atribuído a você)
    Database Password: VMware1!

3\. Clique **Connect**

### Coloque as informações de Login

 ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS15.jpg)

 1\. Digite **student#** (aonde # é o número atribuído a você) para username e **VMware1!** para password.

 2\. Clique **Sign In**

### Album de Fotos

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS16.jpg)

Parabéns, você fez o login com sucesso no aplicativo de fotos!

OPCIONAL: Sinta-se à vontade para tirar uma foto da sala com o seu smartphone e enviá-lo para a pasta Pública.

Em resumo, o front-end (servidor da web) está sendo executado no VMware Cloud on AWS como uma Máquina Virtual, o back-end que é um banco de dados MySQL que está sendo executado no AWS Relational Database Service (RDS) e se comunicando pela Elastic Network Interface (ENI) que é estabelecida com a criação do SDDC.

Você completou o laboratório. Obrigado!
