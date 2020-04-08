---
layout: single
title: "Site Recovery Manager (SRM) Lab Manual"
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
categories: labs
---
# Introducción

En este módulo, se emparejará con otro grupo de alumnos para simular las tareas de configuración de VMware Site Recovery Manager con VMware Cloud on AWS.

## Activación del Site Recovery Add On

Instrucciones Importantes para los Ejercicios de Site Recovery

POR FAVOR ASEGURECE DE USAR EL DESKTOP ASIGNADO POR SU INSTRUCTOR PARA REALIZAR ESTOS EJERCICIOS. SI SE INTENTA REALIZAR ALGUNOS EJERCICIOS FUERA SE LA SESIÓN ASIGNADA SE EXPERIMENTARÁN FALLAS. POR FAVOR USE EL ESCRITORIO ASIGNADO DESDE EL COMIENZO DEL TALLER.

### Activación del Site Recovery Add On

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM1.jpg)

1\. En el SDDC asignado, haga click en la pestaña *Add Ons*

2\. Haga click en el botón *Activate*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM2.jpg)

3\. Haga click en el *Activate*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM3.jpg)

Espere a que se active el Site Recovery Add On.

### ¿Qué es VMware Site Recovery?

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM4.jpg)

VMware Site Recovery trae las capacidades empresariales de Software-Defined Data Center (SDDC) Disaster Recovery al Servicio de VMware Cloud on AWS. Site Recovery permite que los clientes protejan y recuperen aplicaciones sin necesidad de un sitio alterno dedicado. Es entregado, vendido, soportado, mantenido y administrado por VMware como un servicio on-demand. El equipo de TI administra los recursos en la nube con las herramientas de VMware con las que ya son familiares sin necesidad de aprender nuevas herramientas o uso de nuevas herramientas.

VMware Site Recovery es un add-on a VMware Cloud on AWS, que hace uso de VMware Cloud Foundation, VMware Cloud on AWS integra los mejores productos de cómputo, almacenamiento y virtualización de redes de VMware: VMware vSphere, VMware vSAN y VMware NSX junto con VMware vCenter Server para la administración, optimizándolas para correr en la infraestructura física y elástica de AWS. Con la misma arquitectura y experiencia operacional tanto en el centro de datos como en la nube, los equipos de TI ahora pueden obtener valor de inmediato al usar AWS y la experiencia de nube híbrida de VMware.

La solución VMware Cloud on AWS le permite a los clientes tener la flexibilidad para utilizar la nube privada y la nube pública sin distinción y transferir fácilmente cargas de trabajo entre ellas, por ejemplo: mover aplicaciones desde DevTest a producción o incrementar rápidamente su capacidad. Los usuarios pueden aprovechar la presencia global de AWS mientras obtiene los beneficios de elasticidad de los clusters de SDDC elásticamente escalables, una única factura de VMware por todos los servicios de software altamente integrados más la infraestructura AWS, y servicios on-demand o por suscripción como VMware Site Recovery Service.

VMware Site Recovery extiende VMware Cloud on AWS para proveer recuperación de desastres administrado, prevención de desastres y capacidades de pruebas no disruptivas para los usuarios de VMware sin necesidad de un sitio secundario o una configuración compleja.

VMware Site Recovery funciona junto con VMware Site Recovery Manager 8.0 y VMware vSphere Replication 8.0 para automatizar el proceso de de recuperación, pruebas, re-protección y recuperación de las cargas de trabajo virtuales.

VMware Site Recovery utiliza servidores de VMware Site Recovery Manager para coordinar las operaciones del SDDC de VMware. Esto significa que las máquinas virtuales en el sitio protegido son apagadas, las copias de las máquinas virtuales en el sitio de recuperación se encenderán. Al usar la información replicada desde el sitio protegido estas máquinas asumen la responsabilidad de prestar los mismos servicios.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM5.jpg)

VMware Site Recovery puede ser usado en los centro de datos de los usuarios y un SDDC implementado en VMware Cloud on AWS ó puede ser usado entre dos SDDCs implementados en dos zonas de disponibilidad diferentes o regiones de AWS. Esta segunda opción le permite a VMware Site Recovery proveer un servicio completamente administrado por VMware y una solución de Disaster Recovery gestionada.

La migración del inventario protegido y servicios de un sitio a otro esta controlado por un plan de recuperación que especifica el orden en el cual las máquinas virtuales son apagadas y encendidas, los grupos de recursos en los cuales residen, y las redes a las que tienen acceso. VMware Site Recovery permite la prueba de los planes de recuperación, usando una copia temporal de la información replicada y en redes aisladas de manera que no interrumpa las operaciones en ninguno de los sitios. Multiples planes de recuperación pueden ser configurados para mirar aplicaciones individuales o sitios completos al proveer un control mas granular sobre las maquinas virtuales que fallaron y fueron recuperadas. Esto también permite mayor flexibilidad en los horarios de recuperación.

VMware Site Recovery extiende las caraterísticas de una plataforma de infraestructura virtual que permiten una mayor continuidad de negocio a pesar de fallas parciales o completas de un sitio.

## Creación de una Cross SDDC VPN

Se realizará la configuración de una conexión usando una VPN IPSEC entre su VPC y el VPC de su compañero.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM6.jpg)

1\. Regrese a la pestaña *VMware Cloud on AWS*

2\. En la ventana principal del SDDC, haga click en *View Details*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM7.jpg)

3\. Luego haga click en el menú *Network*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM8.jpg)

En el campo Management Gateway, tome nota de la *Public IP* y del *Infrastructure Subnet CIDR*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM9.jpg)

Baje un poco para ver la configuración del Management Gateway

4\. Haga click en la flecha desplegable para abrir la sección *IPsec VPNs*

5\. Haga click en *ADD VPN*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM10.jpg)

Complete la siguiente información

6\. Student # MGMT GW : Escriba el número de estudiante de su compañero

7\. La dirección *IP Pública* de su compañero

8\. El *Infrastructure IP CIDR* de su compañero

9\. La Pre-shared key es **VMware1!**

10\. Haga click en *Save*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM11.jpg)

Cuando usted y su compañero hayan completado los pasos deberían ver el estado como **Connected**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM12.jpg)

Es necesario crear una segunda VPN en nuestro Host para que esta configuración funcione. Normalmente no es necesario cuando se configura un ambiente on-premises pero en este caso en especial se debe hacer.

11\. Asegúrese que el menu desplegable este abierto, si no haga click en *Management Gateway*

12\. Haga click en *Add VPN*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM13.jpg)

Complete con la siguiente información

13\. Dele el nombre a esta VPN de **Student# to Host** donde # es su número de estudiante

14\. Escriba **54.70.191.234** como el Remote Gateway Public IP

15\. Escriba **192.168.30.0/24** en Remote Networks

16\. La Pre-shared key es **VMware1!**

17\. Haga click en *Save*

## Preparación y Conexión de Site Recovery

### Reglas de Firewall para Site Recovery

Para este ejercicio, utilizaremos la nueva opción de *Firewall Rule Accelerator* en el portal de VMware Cloud on AWS.

El Firewall Rule Accelerator creará un grupo de reglas de Firewall para ciertos casos de uso. La red remota del VPN seleccionado será utilizado como la fuente y destino para estas reglas. Usted puede editar estas reglas en la sección de Firewall Rules después que hayan sido creadas por la herramienta si es deseado, aunque esto no es necesario para este ejercicio.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM14.jpg)

1\. Haga click en la pestaña llamada *Network*

2\. Expanda el aérea llamada *Firewall Rule Accelerator*

3\. Seleccione la opción de *Site Recovery* para el *Rule Group*

4\. Para la opción de VPN, seleccione el VPN creado con el estudiante designado como su pareja para este ejercicio, también repetirá el mismo proceso para el VPN creado para la infrastructure Host.

**Asegúrese de repetir este paso tanto para el VPN creado con el ambiente de su compañero como con el VPN de el ambiente Host.**

5\. Haga click en el botón *Create Firewall Rules*

Observe como esta herramienta crea automáticamente todas las reglas de Firewall. Una vez completadas, favor de repetir los mismos pasos en el segundo VPN creado, una vez este segundo haya terminado, revise las reglas de Firewall creadas.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM15.jpg)

### VMware Site Recovery - Conexión con el Sitio

*NOTA IMPORTANTE*: Solo una persona puede hacer el ejercicio de Site Pairing. Decidan quien realizará este paso.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM16.jpg)

1\. En el Portal VMware Cloud on AWS haga click en la pestaña *Add Ons*

2\. Haga click en *Open Site Recovery*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM17.jpg)

3\. Haga click en *New Site Pair*

Se conectará al sitio asignado por su instructor, note que ha sido usada para su SDDC hasta ahora.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM18.jpg)

Esta es la información que su compañero necesitara de usted y que usted tendra obtendrá del sitio de su compañero.

4\. Haga click en la pestaña *Settings*

El usuario de los dos lados (su compañero y suyo) siempre será *cloudadmin@vmc.local*

5\. Copie o escriba la contraseña

6\. Tome nota del URL del vCenter Server, favor anotar el formato desplegado versus el formato a usar:

*DESPLEGADO*: https://vcenter.sddc-xx-xxx-xx-xx.vmc.vmware.com/ui

*A USAR*: vcenter.sddc-xx-xxx-xx-xx.vmc.vmware.com

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM19.jpg)

7\. Asegúrese que el vCenter local este seleccionado

8\. Escriba la información del SDDC de su compañero:

    PSC host name

    User name

    Password

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM20.jpg)

9\. Asegúrese que el vCenter server local este seleccionado

10\. Seleccione *All Services*

11\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM21.jpg)

12\. Haga click en el botón *Finish*

### Configuración de los Network Mappings

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM22.jpg)

13\. Haga click en *Network Mappings* en el panel izquierdo en la página Site Recovery

14\. Haga click en *+ New*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM23.jpg)

15\. Seleccione *Prepare mappings manually*

16\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM24.jpg)

17\. Expanda *SDDC Datacenter* en los dos sitios

18\. Expanda *Management Networks* en los dos sitios

19\. Expanda *vmc-dvs* en los dos sitios

20\. Seleccione su red *Student#-LN* y la red *Student#-LN* de su compañero (Podría ser necesario buscarla en la lista)

21\. Haga click en el botón *Add Mappings*

22\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM25.jpg)

23\. NO ESCRIBA o seleccione nada en Reverse Mappings, haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM26.jpg)

24\. Deje los valores por defecto y haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM27.jpg)

25\. Haga click en *Finish*

### Folder mappings

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM28.jpg)

26\. Seleccione *Folder Mappings* en el panel izquierdo

27\. Haga click en *+ New* para crear un nuevo folder mapping

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM29.jpg)

28\. Seleccione *Prepare mappings manually*

29\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM30.jpg)

30\. Expanda *SDDC Datacenter* en los dos sitios

31\. Seleccione *Workloads* en los dos sitios

32\. Haga click en el botón *Add Mappings*

33\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM31.jpg)

34\. NO SELECCIONE ningún Reverse mappings, haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM32.jpg)

35\. Haga click en *Finish*

### Resource Mappings

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM33.jpg)

36\. Haga click en *Resource Mappings* en el panel izquierdo

37\. Haga click en *+ New*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM34.jpg)

38\. Expanda *SDDC Datacenter* en los dos sitios

 39\. Expanda *Cluster 1* en los dos sitios

 40\. Seleccione *Compute-ResourcePool* en los dos sitios

41\. Haga click en el boton *Add Mappings*

 42\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM35.jpg)

43\. NO SELECCIONE ningún reverse mappings, haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM36.jpg)

44\. Haga click en *Finish*

### Storage Policy Mappings

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM37.jpg)

45\. Seleccione *Storage Policy Mappings* en el panel izquierdo

46\. Haga click en *+ New*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM38.jpg)


47\. Seleccione *Prepare mappings manually*

48\. Haga click en *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM39.jpg)

49\. Escoja *Datastore Default* de los dos lados

50\. Haga click en *ADD MAPPINGS*

51\. Haga click en *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM40.jpg)

52\. Seleccione *DatastoreDefault*

53\. Haga click en *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM41.jpg)

54\. Haga click en *FINISH*

### Placeholder Datastores

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM42.jpg)

55\. Seleccione *Placeholder Datastores* en el panel izquierdo

56\. Haga click en *+ New*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM43.jpg)

57\. Seleccione *WorkloadDatastore*

58\. Haga click en *Add*

## SRM - Protección de una VM

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM44.jpg)

1\. Seleccione una VM para replicar y haga click derecho

2\. Seleccione *All Site Recovery actions*

 3\. Haga click en *Configure Replication*

*NOTA*: Podría ser necesario ingresar en sitio conectado (El sitio de su compañero), asegúrese de usar cloudadmin@vmc.local y use la contraseña de su compañero. Luego de entrar podría tener que repetir este paso.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM45.jpg)

4\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM46.jpg)

5\. Seleccione el Target Site

6\. Si ha ingresado es necesario hacerlo (Recuerde es que el sitio de su compañero no el suyo)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM47.jpg)

7\. Escriba las credenciales del sitio de su compañero

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM48.jpg)

8\. Deje los valores por defecto y haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM49.jpg)

9\. Deje *Datastore Default* como la VM Storage Policy por defecto

10\. Seleccione *WorkloadDatastore*

 11\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM50.jpg)

12\. Deje el valor por defecto de 1 hora para Recovery Point Objective, RPO puede ser tan bajo como 5 minutos, y tan alto como 24 horas

13\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM51.jpg)

14\. Seleccione *Add to new protection group* 

15\. Dele el nombre *PG#* a su Protection Group donde # es su número de estudiante

16\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM52.jpg)

17\. Seleccione *Add to new recovery plan*

 18\. Dele el nombre de **RP#** a su Recovery Plan donde # es su número de estudiante

19\. Haga click en el boton *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM53.jpg)

20\. Haga click en *Finish*

## Realice una Prueba de Recuperación

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM54.jpg)

1\. En el portal VMware Cloud on AWS portal, haga click en la pestaña *Add Ons*

2\. Haga click en *Open Site Recovery* (Se deben habilitar las ventanas emergentes)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM55.jpg)

3\. En la ventana Site Recovery, haga click en *Open*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM56.jpg)

4\. Haga click en *Recovery Plans*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM57.jpg)

5\. Haga click en su grupo de protección *PG#* donde # es su número de estudiante

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM58.jpg)

6\. Haga click en *Recovery Plans* 

7\. Haga click en *RP#* el cual debería ser su Recovery Plan creado en el paso anterior

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM59.jpg)

8\. Haga click en el botón *Test*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM60.jpg)

9\. Deje todos los valores por defecto y haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM61.jpg)

10\. Haga click en el botón *Finish*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM62.jpg)

11\. Siga el progreso de la tarea en *Recent Tasks* que se encuentra en la parte inferior de la ventana

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM63.jpg)

12\. Note que la prueba fue completada exitosamente

13\. Haga click en el botón *Cleanup* para limpiar la actividad y devolver el ambiente a su estado normal

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM64.jpg)

14\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM65.jpg)

15\. Haga click en *Finish*, el ambiente ahora está limpio. Note que durante la prueba, las replicaciones que protegen sus VMs no son interrumpidas
