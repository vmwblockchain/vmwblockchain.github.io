---
layout: single
title: "vRealize Automation Lab Manual"
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
---
# Introducción

Muy a menudo, los clientes desean utilizar herramientas que se encuentran por encima de la capa API de vCenter y proporcionan un valor adicional al proceso de consumo o monitoreo. Una de estas herramientas es VMware vRealize Automation, que se puede utilizar para crear un portal de gobierno y "tienda frontal" para que los usuarios puedan aprovisionar cargas de trabajo donde lo deseen, según las políticas definidas por el administrador de la nube.

## ¿Qué es vRealize Automation (vRA)?

VMware vRealize Automation es la herramienta de Automatización IT para Software-Defined Data Center.

vRealize Automation permite la Automatización IT a través de la creación y administración de infraestructura personalizada, aplicaciones y servicios IT avanzados (XaaS). Esta clase de Automatización IT permite implementar servicios IT rápidamente en infraestructura de múltiples fabricantes o múltiples nubes.

Lo qué vRealize Automation Entrega:

• Agilidad a través de la Automatización de IT

  Acelere la entrega completa de servicios y la gestión de infraestructura y aplicaciones.

• Selecciones a través de la Flexibilidad

  Aprovisione y administre infraestructura de múltiples fabricantes o nubes y aplicaciones al hacer uso de infraestructura, herramientas y procesos nuevos o existentes.

• Control a través de Políticas de Gobierno

  Asegure que los usuarios reciban los recursos adecuados o aplicaciones con el nivel apropiado de servicio de acuerdo a sus necesidades.

• Ahorro de Costos a través de la Eficiencia

  Reduce el costo operacional al reducir procesos manuales o que consumen gran cantidad de tiempo e incrementando los ahorros a través de la reclamación automatizada de recursos inactivos.

vRealize Automation soporta VMware Cloud on AWS al entregar una experiencia de administración de nube híbrida.

## Ingreso a vRealize Automation


![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA1.jpg)

1\. En el Escritorio de Estudiante, haga click in Chrome.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA2.jpg)

2\. En la barra de accesos directos en Chrome haga click en el enlace *VMware vRealize Automation Appliance*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA3.jpg)

3\. Haga click en el enlace *Proceed to vraapp.corp.local (unsafe)*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA4.jpg)

5\. Haga click en el enlace *vRealize Automation Console*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA5.jpg)

6\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA6.jpg)

7\. Ingrese con el usuario *vmcws#* donde # es el número de estudiante

8\. Escriba la contraseña **VivaVMConAWS!**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA7.jpg)

Ha ingresado exitosamente a vRealize Automation!

## Agregue un vSphere Endpoint

Un *endpoint* es cualquier cosa que vRealize Automation usa para completar los procesos de aprovisionamiento. Pueden ser recursos de nube pública como Amazon Web Services EC2, VMware Cloud on AWS, una aplicación de orquestación externa, o una nube pública basada en vSphere u otros hipervisores.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA8.jpg)

1\. Haga click en la pestaña *Infrastructure* 

2\. Haga click en *Endpoints* en el panel izquierdo

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA9.jpg)

3\. Haga click en *Endpoints* de nuevo en el panel izquierdo

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA10.jpg)

4\. Haga click en el boton *New*

 5\. Seleccione *Virtual*

 6\. Seleccione *vSphere (vCenter)*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA11.jpg)

En estos siguientes pasos, asegúrese de haber ingresado al portal VMware Cloud on AWS que le fue asignado.

7\. En el portal VMware Cloud on AWS, asegúrese de hacer click en la pestaña *Settings*, ya que se usará información de esta sección para crear el vRealize Automation Endpoint

8\. Dele el nombre de **ws#** al vRA Endpoint donde # es su número de estudiante

9\. Bajo *Address* escriba la información que esta en el portal VMware Cloud on AWS portal bajo *vSphere Client (HTML5)* y reemplace la palabra ui por sdk

10\. Ejemplo:

 Para el portal VMware Cloud on AWS: https://vcenter.sddc-X-X-X-X.vmc.vmware.com/ui 

Se debería ver en vRealize Automation así: https://vcenter.sddc-X-X-X-X.vmc.vmware.com/sdk

11\. Escriba el nombre de usuario **cloudadmin@vmc.local**

 12\. Escriba la contraseña (Por favor revise esta información en el portal VMware Cloud on AWS)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA12.jpg)

13\. Haga click en el boton *Test Connection*

14\. Si la conexión termina la prueba exitosamente, haga click en el boton *Ok* para crear el vRealize Automation Endpoint

## Create a Fabric Group

Un administrator puede agrupar los recursos de cómputo asignados a la virtualización y los cloud endpoints en *fabric groups* por tipo o uso.

Asegúrese que para este paso haya seleccionado el Endpoint que fue creado en el módulo anterior.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA13.jpg)

1\. Haga click en *Fabric Groups* en el panel izquierdo

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA14.jpg)

2\. Haga click en *+ New* para crear un nuevo Fabric Group

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA15.jpg)

3\. Dele el Nombre de **ws#FabricGroup** al Fabric Group donde # es su número de estudiante

4\. Agregue el usuario asignado a su número de estudiante: *vmcws#@corp.local* y haga click en el icono lupa para encontrar y agregar el usuario

5\. Seleccione el vRealize Automation endpoint creado en el paso anterior

  *NOTA*: ASEGURESE DE SELECCIONAR EL RECURSO DE CÓMPUTO COMO SE ILUSTRA EN LA IMAGEN . DEBE SER EL ENDPOINT *WS#* CREADO EN EL PASO ANTERIOR.

6\. Haga click en el boton *Ok*

## Reservaciones

Cuando un usuario solicita una máquina, puede ser aprovisionada en cualquier reservación del tipo apropiado y que tenga la capacidad suficiente. Es posible aplicar una politica de reservación a un blueprint para restringir la cantidad de maquinas aprovisionadas de ese blueprint a un subconjunto de reservas disponibles.

Para este paso si la pestaña *Reservation* no aparece es posible que sea necesario actualizar la pagina en el browser.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA16.jpg)

1\. Haga click en el botón *< Infrastructure* en el panel izquierdo

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA17.jpg)

2\. Haga click en *Reservations* en el panel izquierdo

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA18.jpg)

3\. Haga click en *Reservation Policies* en el panel izquierdo

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA19.jpg)

1\. Haga click en el botón *+ New*

 2\. Dele el nombre de *Student#* a la Reservation Policy

3\. Haga click en el boton *OK*

### Creación de una Reservación

Una reservación de vRealize Automation es un medio para asignar recursos de un fabric group (CPU, RAM, Storage, etc.) a un business group especifico.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA20.jpg)

1\. Haga click en el botón *< Infrastructure* en el panel izquierdo

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA21.jpg)

2\. Haga click en *Reservations* en el panel izquierdo

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA22.jpg)

3\. Haga click en *Reservations* en el panel izquierdo

 4\. Haga click en *+ New* para crear una nueva Reservation

5\. Seleccione *vSphere (vCenter)*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA23.jpg)

6\. Dele el nombre de **Student#** a la reservación, donde # es el número de estudiante asignado

7\. Deje/seleccione *vsphere.local* como Tenant

 8\. Seleccione *vmc-ws#* donde # es su número de estudiante

9\. Como Reservation policy escriba o seleccione *Student#*

10\. Priority debería estar en *1* 

11\. Haga click en la pestaña *Resources*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA24.jpg)

12\. En la pestaña *Resources* para *Compute Resource* seleccione *Cluster-1 (WS#)* en la caja desplegable

13\. Escriba **1024** en el campo *This Reservation*

 14\. Seleccione la caja de verificación *WorkloadDatastore*

 15\. Escriba **10000** en el campo *This Reservation Reserved*

 16\. Escriba **0** en el campo Priority field y haga click en el boton *OK*

17\. Seleccione *Compute-ResourcePool* en el campo *Resource Pool*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA25.jpg)

18\. Haga click en la pestaña *Network*

19\. Seleccione el Network Adapter *sddc-cgw-network-1* haciendo click en la caja de verificación en la izquierda y seleccione *sddc-cgw-network-1* en la caja desplegable en el lado derecho

20\. Haga click en el botón *OK*

## Creación de un Blue Print

Un blueprint es la especificación completa de un servicio. Un blueprint determina los componentes de un servicio los cuales son parámetros de entrada, cuales son formularios de solo lectura o de solicitud, secuencia de acciones, y aprovisionamiento. Se pueden crear blueprints de servicio para aprovisionar recursos personalizados que hayan sido creados previamente.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA26.jpg)

1\. Haga click en la pestaña *Design*

 2\. En el panel izquierdo haga click en *Blueprints*

3\. Haga click en el boton *+ New*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA27.jpg)

4\. Dele el nombre **Student#** al Blueprint donde # es el número de estudiante asignado

5\. Use el mismo nombre como ID 

6\. Escriba **1** como Deployment Limit

 7\. Escriba **1** como Minimum Lease days

8\. Escriba **1** como Maximum Lease days 

9\. Escriba **0** en el campo Archive Days 

10\. Haga click en el botón *OK*, entonces aparecerá el Design Canvas

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA28.jpg)

11\. Asegúrese que *Machine Types* este seleccionado

 12\. Haga click y arrastre al Canvas una *vSphere (vCenter) Machine*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA29.jpg)

13\. Deje los valores por defecto en la pestaña *General* haga click en la pestaña *Build Information*

14\. Asegúrese *Server* este seleccionado como Blueprint Type

15\. Cambie Action a *Clone*

16\. Haga click en botón con puntos suspensivos y seleccione *StudentVM#* que fue creada en el paso anterior y haga click en *OK*

17\. Haga click en *Save*

18\. Haga click en *Finish*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA30.jpg)

19\. Seleccione su Blueprint y márquelo

 20\. Haga click en el botón *Publish* para publicar su blueprint

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA31.jpg)

21\. Asegúrese de estar en la pestaña *Administration*, si no, haga click en la pestaña y seleccione *Catalog Management*

22\. Seleccione *Catalog Items* en el panel izquierdo y haga click en *Student#* para configurar

23\. Seleccione *Desktops* en la caja desplegable que esta al lado de *Service* 

24\. Haga click en el *OK*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA32.jpg)

### Asignación de permisos a su Blueprint 

25\. Haga click en la pestaña *Administration*

 26\. Seleccione en el panel izquierdo *Catalog Management*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA33.jpg)

27\. Haga click en *Entitlements* en el panel izquierdo

 28\. Haga click en el botón *+ New* para crear un nuevo Entitlement

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA34.jpg)

29\. Dele el nombre **ws#** al Entitlement donde # es número de estudiante

30\. Cambie Status a *Active*

31\. Seleccione como Business Group *vmc-ws#* donde # es el número asignado

32\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA35.jpg)

33\. Haga click en el signo *+* al lado de *Entitled Services* 

34\. Asegúrese de seleccionar la caja de verificación al lado de *Desktops*

35\. Haga click en *OK*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA36.jpg)

36\. Haga click en el signo *+* al lado de *Entitle Actions*

 37\. Seleccione la caja de verificación en la parte superior para seleccionar todas las Acciones

38\. Haga click en el botón *OK* 

39\. Haga click en el botón *Finish*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA37.jpg)

### Seleccione un articulo del catalogo

40\. Seleccione la pestaña *Catalog*

41\. Haga click en el blueprint *Student#* que acaba de ser creado y haga click en el botón *Request*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA38.jpg)

42\. Seleccione *vSphere_VCenter_Machine* debajo de su Blueprint 

43\. Revise que todas las opciones sean correctas y haga click en el botón *Submit*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/vra/vRA39.jpg)

44\. Haga click en la pestaña *Requests* y revise el estado de la solicitud del Blueprint

45\. Revise el estado para asegurar que la solicitud se complete exitosamente
