---
layout: single
title: "AWS Integration Workshop Manual"
categories: labs
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
categories: labs
---

# Introducción

Una de las razones más convincentes para adoptar VMware Cloud on AWS es integrar sus sistemas existentes que se encuentran en su entorno de nube VMware, con plataformas de aplicaciones que residen en su entorno de nube privada virtual (VPC) de AWS. La integración que VMware y AWS han creado permite que estos servicios se comuniquen, de forma gratuita, a través de un espacio de direcciones de red privada para servicios como las instancias EC2, que se conectan a subredes dentro de una AWS VPC nativa o con servicios de plataforma que tienen la capacidad de conectarse a un punto final de VPC, como almacenamiento S3. En este módulo, trabajaremos en algunas integraciones básicas comunes que puede utilizar en su plataforma VMware Cloud on AWS.

## Entendiendo las integraciones con servicios de AWS

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-1.jpg)

Como el dibujo de arriba demuestra, el software de VMware no solo está posicionado adjunto a servicios de AWS, pero también está integrado herméticamente con estos servicios. Esta integración introduce una nueva manera de pensar en cómo diseñar e influenciar servicios de AWS con su SDDC de VMware. Algunas integraciones que algunos clientes utilizan incluyen:

* Interfaz de VMware con un backend de RDS
* Backend VMware e interfaz en EC2
* AWS Application Load Balancer (ELBv2) con interfaz en VMware (apuntando hacia IPs privadas)
* Lambda, Simple Queueing Service (SQS), Simple Notification Service (SNS), S3, Route53, and Cognito
* AWS Lex, and Alexa con APIs de VMware Cloud

Estos son solo algunos de los ejemplos de integraciones que hemos visto. Existen muchos diferentes servicios que pueden ser integrados con su ambiente.
En este ejercicio estaremos explorando integraciones con AWS Simple Storage Service (S3) y con AWS Relational Database Service (RDS).

**Nota: hay un requisito en este módulo de haber completado los pasos en el módulo [Trabajando con un SDDC](https://vmc-field-team.github.io/labs-es/working-with-sddc-lab/) en relación a la creación de un  Content Library, Redes, y las reglas de Firewall.**

### Cómo son posibles estas integraciones?

Además de existir en la infraestructura de AWS, existe también un Elastic Network Interface (ENI) el cual conecta VMware Cloud on AWS y el Virtual Private Cloud (VPC) del cliente, proveyendo una conección rápida, con latencia baja, entre el VPC y el SDDC. Aquí es donde el tráfico circula entre las dos tecnologías (VMware y AWS). No hay cargo de Egress (transmición de datos) a través del ENI entre la misma zona de disponibilidad y existen Firewalls de los dos lados de esta conección para propósitos de seguridad.

### Cómo se asegura el tráfico a través del ENI?

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-2.jpg)

De el lado de VMware (mire gráfica abajo), el ENI entra al SDDC en el NSX Edge Gateway. Esto significa, que de ese lado de la tecnología se permite o no, el tráfico desde el ENI con reglas de firewall de NSX. Por defecto, tráfico del ENI no está permitido hacia el SDDC. Considere esto como una barrera de seguridad, bloqueando el tráfico hacia o desde los servicios de AWS en el ENI hasta que las reglas sean modificadas.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-3.jpg)

Desde el lado de AWS (mire gráfica abajo), Security Groups son utilizados. Si no está familiarizado con Security Groups, estos actúan como un firewall virtual para diferentes servicios (VPCs, Bases de Datos, instacias de EC2, etc.) de AWS. Estos deben ser configurados para no permitir el tráfico desde y hacia el SDDC de VMware sólo que se desee permitir el tráfico.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-4.jpg)

En este ejercicio, ya todo ha sido configurado de el lado de AWS. Aún así observaremos como el tráfico de AWS se puede permitir para que entra y salga del SDDC de VMware Cloud on AWS.

### Reglas de Firewall en el Compute Gateway para servicios nativos de AWS

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-5.jpg)

1. En el portal de VMware Cloud on AWS haga click en la pestaña de **Networking & Security**
2. Haga click en **Groups** en el panel izquierdo
3. Haga click en **ADD GROUP**

#### Nombre el Workload Group

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-6.jpg)

1. Escriba **PhotoAppVM** como Nombre
2. Asegúrese que **Virtual Machine** esté seleccionada como Member Type
3. Haga click en **Set VMs** bajo Members

#### Selecione VMs - Workload Group

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-7.jpg)

1. Haga click para seleccionar **Webserver01**
2. Haga click en **SAVE**

#### Save Group - Workload Group

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-8.jpg)

1. Click **SAVE**

### Reglas de Firewall

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-9.jpg)

1. Haga click en la pestaña de **Networking & Security** en el portal de VMware Cloud on AWS
2. Haga click en **Gateway Firewall** en el panel izquierdo
3. Haga click y seleccione **Compute Gateway**
4. Haga click en **ADD NEW RULE**

#### Crear Nueva Regla - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-10.jpg)

1. Nombre su regla **AWS Inbound**
2. Haga click en **Set Source**

#### Seleccione Origen - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-11.jpg)

1. Haga click y seleccione **Connected VPC Prefixes**
2. Haga click en **SAVE**

#### Marcar el destino - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-12.jpg)

1. Haga click en **Set Destination**

#### Seleccione el destino - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-13.jpg)

1. Haga click y seleccione **PhotoAppVM**
2. Haga click en **SAVE**

#### Marcar el Servicio - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-14.jpg)

1. Haga click en **Set Service**

#### Marcar el servicio - AWS Inbound (Continuación)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-15.jpg)

1. Haga click y seleccione **Any**
2. Haga click en **SAVE**

#### Publicar - AWS Inbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-16.jpg)

1. Haga click en **PUBLISH**

**Nota:** Asegúrese de dejar **All Uplinks** seleccionado en la sección de **Applied To**

#### Agregar nueva regla - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-17.jpg)

1. Haga click en **ADD NEW RULE**
2. Nombre su regla **AWS Outbound**
3. Haga click en **Set Source**

#### Seleccionar Origen - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-18.jpg)

1. Haga click y seleccione **PhotoAppVM**
2. Haga click en **SAVE**

#### Marcar Destino - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-19.jpg)

1. Haga click en **Set Destination**

#### Seleccionar Destino - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-20.jpg)

1. Haga click y seleccione **Connected VPC Prefixes**
2. Haga click en **SAVE**

#### Marcar Servicio - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-21.jpg)

1. Haga click en **Set Service**

#### Marcar Service - AWS Outbound (Continuación)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-22.jpg)

1. Bajo **Select Services** escriba **3306**
2. Seleccione **MySQL** checkbox
3. Haga click en **SAVE**

#### Publicar - AWS Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-23.jpg)

1. Haga click en **PUBLISH**

**Nota:** Asegúrese de dejar **All Uplinks** en la sección **Applied To**

### Agregar Nueva Regla - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-24.jpg)

1. Haga click en **ADD NEW RULE**

#### Agregar Nueva Regla - Public In (Continuación)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-25.jpg)

1. Escriba **Public In** como Nombre
2. Haga click en **Set Source**

#### Seleccionar Origen - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-26.jpg)

1. Haga click y seleccione **Any**
2. Haga click en **SAVE**

#### Marcar Destino - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-27.jpg)

1. Haga click en **Set Destination**

#### Seleccionar Destino - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-28.jpg)

1. Haga click y seleccione **PhotoAppVM**
2. Haga click en **SAVE**

#### Marcar Servicio - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-29.jpg)

1. Haga click en **Set Service**

#### Marcar Servicio - Public In (Continuación)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-30.jpg)

1. Escriba **HTTP 80** bajo **Select Services**
2. Haga click y seleccione **HTTP**
3. Haga click en **SAVE**

#### Publicar - Public In

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/AWS-31.jpg)

1. Haga click en **PUBLISH**

**Nota:** Asegúrese de dejar **All Uplinks** en la sección de **Applied To**

## Integración con AWS Relational Database Service (RDS)

Amazon RDS facilita la configuración, el funcionamiento y la escala de una base de datos relacional en la nube. Proporciona una capacidad rentable y redimensionable a la vez que automatiza las tareas de administración que requieren mucho tiempo, como el aprovisionamiento de hardware, la configuración de la base de datos, la aplicación de parches y las copias de seguridad. Le permite concentrarse en sus aplicaciones para que pueda brindarles el rendimiento rápido, la alta disponibilidad, la seguridad y la compatibilidad que necesitan.

En este ejercicio, se podrá integrar una máquina virtual de Vmware Cloud on AWS para trabajar en conjunto con una base de datos relacional que se ejecuta en Amazon Web Services (AWS) que ha sido configurada previamente para usted.

### Tome nota de la dirección IP de Webserver01

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS1.jpg)

Se utilizará la máquina virtual creada en el módulo anterior para pode completar este ejercicio.

1. En la interface de vCenter de VMware Cloud on AWS, encuentre la VM desplegada en el ejercicio pasado llamada **Webserver01**, asegúrese que se le haya asignado una dirección IP como en la gráfica.

### Asignar IP pública

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS2.jpg)

1. Regrese al portal de VMware Cloud on AWS y haga click en la pestaña de **Networking & Security** para poder solicitar una IP pública
2. Haga click en **Public IPs** en el panel izquierdo
3. Haga click en **REQUEST NEW IP**
4. En el área de notas escriba **PhotoAppIP**
5. Haga click en **SAVE**

### Tome nota de la nueva IP pública

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS3.jpg)

Tome nota de la nueva IP pública asignada.

### Crear una Regla NAT

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS4.jpg)

1. Haga click en **NAT** en el panel izquierdo
2. Haga click en **ADD NAT RULE**
3. Escriba **PhotoApp NAT** como Nombre
4. Asegúrese que la IP pública asignada en el paso anterior aparezca bajo Public IP
5. Deje seleccionado **All Traffic** (no se cambia)
6. Escriba la dirección IP de su VM **Webserver01** que anotó al iniciar este ejercicio
7. Haga click en **SAVE**

## Integración con AWS Relational Database Service (RDS)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS24.jpg)

En su buscador, abra una pestaña nueva y entre a este enlace: https://vmcworkshop.signin.aws.amazon.com/console

1.. Account ID o alias - Por favor utilice la información en la tarjeta proveída para determinar la información de su Account ID
2. IAM user name - **Student#** (donde # es el número que le fue asignado)
3. Password - **VMCworkshop1211**
4. Haga click en **Sign In**

Por favor note que puede que reciba 2 pantallas de Sign In en este paso. Si recibe la del lado derecho, entre Account ID y haga click en **Next**

### Información de RDS

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS6.jpg)

1. Una vez conectado a la consola de AWS, asegúrese que haya seleccionado la región de **Oregon**
2. Haga click en el servicio **RDS** (Pueda que necesite expandir **All services**)

### Instancia RDS

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS07.jpg)

1. En el panel izquierdo haga click en **Databases**
2. Haga click en la instancia RDS que corresponde a su número designado

### Navegar a Security Groups

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS08.jpg)

1. Baje hasta el área **Details** y bajo **Connectivity & Security** note que la instancia de RDS no es accesible públicamente, lo que significa que esta instacia solo puede ser accedida desde el interior de AWS
2. Haga click en el enlace azul bajo **Security Groups**

### Security Groups

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS9.jpg)

1. Escoja el RDS Security Group **Student##-RDS-Inbound** que le corresponda a usted (puede que no sea su número de estudiante)
2. Después de escoger el security group apropiado, haga click en la pestaña **Inbound**

**Nota: VMware Cloud on AWS establece rutas en el VPC por defecto, solo RDS puede influenciar estas o crear nuevas**

### Tráfico Outbound

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS10.jpg)

1. Haga click en la pestaña **Outbound**
2. Puede observar todo el tráfico (interno a AWS) es permitido, esto incluye el SDDC de VMware Cloud on AWS y los segmentos de red lógicos que existen en el SDDC.

### Elastic Network Interface (ENI)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS11.jpg)

El AWS Relational Database Service (RDS), también crea su propio Elastic Network Interface (ENI) para tener acceso, el cual es un ENI distinto al creado por VMware Cloud on AWS.

1. Haga click en **Services** para regresar a la consola principal
2. Haga click en **EC2**

### ENI (Continuación)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS12.jpg)

1. En el dashboard de EC2 haga click en **Network Interfaces** en el panel izquierdo
2. Todos los ambientes de estudiantes pertenecen a la misma cuenta de AWS, por ende, cientos de ENIs pueden existir. Para poder hacer más fácil la búsqueda, escriba **RDS** en el área de búsqueda y oprima Enter para añadir un filtro
3. Escoja su security group **Student##-RDS-Inbound** que corresponde a su número de estudiante basado en el segundo octect del bloque de IPs en la última columna

    En este ejemplo, el bloque de IPs es 172.6.8.187, esto correspondería al estudiante **6**

4. Tome nota de la dirección **Primary private IPv4 IP** para el siguiente paso

### Photo App

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS13.jpg)

1. En su teléfono inteligente (Tableta o computadora personal), abra un buscador y escriba la IP pública que se pidió en el portal de VMware Cloud on AWS, seguido por /Lychee (distingue mayúsculas y minúsculas),por ejemplo: 1.2.3.4/Lychee
2. Escriba la información de abajo (_sensible a mayúsculas y minúsculas_) para conectarse a la base de datos, utilizando la dirección IP anotada en el paso previo de el ENI de RDS:

    Database Host: x.x.x.x:3306
    Database Username: student# (donde # corresponde a su número de estudiante)
    Database Password: VMware1!

3. Haga click en **Connect**

### Ingresar información de inicio de sesión

 ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS15.jpg)	

1. Escriba **student#** (donde # corresponde a su número asignado) para user name y **VMware1!** como contraseña
2. Haga click en **Sign In**

### Photo Albums

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS16.jpg)

Felicidades, se ha logrado conectar exitosamente al photo app!

OPCIONAL: Tómese la libertad de tomarle una foto al cuarto donde se encuentra con su teléfono y subirla a la carpeta pública.

Resumiendo, la interfaz (servidor web) está corriendo en VMware Cloud on AWS como una máquina virtual (VM), el back end, que es una base de datos MySQL, está corriendo en AWS Relational Database Service y comunicándose a través del Elastic Network Interface (ENI) que se establece en el momento de crearse el SDDC.

Ha completado este ejercicio, gracias por su visita hoy!