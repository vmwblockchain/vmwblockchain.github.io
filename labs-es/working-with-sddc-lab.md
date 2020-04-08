---
layout: single
title: "Working with your SDDC Lab Manual"
date: 2018-07-17
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---
# Introducción

En este módulo, comenzaremos con las tareas básicas que realizará en la interfaz de usuario de VMware Cloud on AWS cuando administre la plataforma.

## Explorando su SDDC

![SDDC-Network-Login](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc-login.jpg)

Habra la consola de VMware Cloud on AWS usando https://vmc.vmware.com y utilice los credenciales asignados, por ejemplo: **ced##@vmware-hol.com**.

Después de iniciar la sesión, debería ver dos SDDC de un solo nodo en la interfaz de usuario siguiendo el formato de nombre de Student-##. Un SDDC es un entorno completamente implementado que incluye vSphere, NSX, vSAN y vCenter Server. La implementación de un SDDC totalmente configurado toma aproximadamente dos horas, por lo que, para los fines de este laboratorio, ya lo hemos implementado para usted. Este SDDC se encuentra en el mismo estado que tendría si lo hubiera implementado usted. Echemos un vistazo a las propiedades del SDDC.

![SDDC-Network-01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc01.jpg)

1. Primero, identifique el SDDC asignado a usted (Student-##).
2. Haga click en **View Details** para abrid las propiedades del SDDC.

![SDDC-Network-02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc02.jpg)

Comenzará con el Resumen del SDDC. Hay una serie de otras pestañas disponibles de la siguiente manera:
1. **Support**: puede ponerse en contacto con Soporte con su ID de SDDC, ID de organización, IP privadas y públicas de vCenter y la fecha de su implementación de SDDC.
2. **Settings**: le da acceso a su cliente vSphere (HTML5), vCenter Server API, PowerCLI Connect, vCenter Server y revisa su información de autenticación.
3. **Troubleshooting**: Le permite ejecutar pruebas de conectividad de red para garantizar que todos los accesos necesarios estén disponibles para realizar casos de uso seleccionados.
4. **Add Ons**: Aquí encontrará los servicios adicionales disponibles para su ambiente de VMware Cloud on AWS como Hybrid Cloud Extension y VMware Site Recovery.
5. **Networking & Security**: proporciona un diagrama completo de las puertas de enlace de administración y cómputo. Aquí es donde puede configurar redes locales, VPN y reglas de firewall. Vamos a cubrir esto con más detalle más adelante. Haga click en **Networking & Security** para continuar con el siguiente ejercicio y obtener más información sobre la configuración de redes y seguridad en VMware Cloud on AWS.

## Creación de un segmento de red

![SDDC-Network-03](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc03.jpg)

Basado en el ejercicio anterior, la pestaña de **Networking & Security** debe estar visible.
VMware Cloud on AWS le permite crear segmentos lógicos de red de una manera fácil y rápida en el momento que lo necesite. Crearemos un segmento nuevo en el SDDC.

1. Haga click en la pestaña **Networking & Security**, luego haga click en **Segments** para mostrar todos los segmentos de red existentes.
2. Haga clic en **Add Segments** para crear un nuevo segmento de red.
3. Escriba **Demo-Net** para el Nombre del nuevo segmento de red.
4. Para la Longitud de la entrada/prefijo, ingrese **10.10.xx.1/24** (xx representa su número de estudiante). Esto representa la puerta de enlace predeterminada de la red y la longitud del prefijo de la red. Para obtener más detalles sobre el direccionamiento IP, consulte la imagen abajo.
5. Para **DHCP**, haga click en la flecha hacia abajo y seleccione **Enabled** para habilitar DHCP en la red.
6. Ingrese **10.10.xx.10-10.10.xx.200** para el **DHCP IP Range**. Este es el rango de direcciones IP que el servidor DHCP otorgará a las cargas de trabajo conectadas a la red.
7. Haga click en **Save** para guardar la red lógica.

**Nota: asegúrese de dejar el valor predeterminado de Routed (enrutado) para tipo y no ingrese nada para el sufijo DNS.**

<aside class="notice">
<font color="dodgerblue">
<img src="https://s3-us-west-2.amazonaws.com/vmc-workshops-images/info.jpeg" width="25" height="25"> La notación CIDR es una representación compacta de una dirección IP y su prefijo de enrutamiento asociado. La notación se construye a partir de una dirección IP, un carácter de barra ('/') y un número decimal. El número es el recuento de bits iniciales en la máscara de enrutamiento, tradicionalmente llamada máscara de red. La dirección IP se expresa de acuerdo con los estándares de IPv4 o IPv6.
<br/>
<br/>
La dirección puede denotar una única dirección de interfaz distinta o la dirección de inicio de una red completa. El tamaño máximo de la red viene dado por el número de direcciones posibles con los bits restantes, menos significativos, debajo del prefijo. La agregación de estos bits a menudo se denomina identificador de host.
<br/>
<br/>
Por ejemplo:
<br/>
<br/>
• 192.168.100.14/24 representa la dirección IPV4 192.168.100.14 y su prefijo de enrutamiento asociado 192.168.100.0, o equivalentemente, su máscara de subred 255.255.255.0, que tiene 24 bits de 1 bit.
<br/>
<br/>
• El bloque IPV4 192.168.100.0/22 representa las 1024 direcciones IPV4 de 192.168.100.0 a 192.168.103.255.
</font>
</aside>

## Verifique la configuración del segmento de red

![SDDC-Network-04](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc04.jpg)

1. Verifique que el segmento de red se haya agregado correctamente. Su información debe coincidir con el área resaltada arriba.

## Configurar la regla de Firewall para acceso a vCenter

![SDDC-Network-05](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc05.jpg)

De forma predeterminada, todas las reglas de firewall de entrada están configuradas para denegar el tráfico en VMware Cloud on AWS. Para poder acceder al servidor vCenter, necesitaremos configurar una regla de firewall que permita el acceso de entrada.

**Nota: En la mayoría de los ambientes empresariales, se crearía un VPN o Direct Connect VIF para permitir las reglas de firewall de acceso limitado a vCenter. En este entorno, lo abriremos en cualquier dirección IP en Internet, algo que no es recomendable.**

1. Haga click en **Gateway Firewall** en el lado izquierdo de la pantalla.
2. Si aún no está seleccionado, haga click en **Management Gateway** para crear reglas de firewall que permitan el acceso a los componentes de administración en el SDDC.
3. Haga click en **Add New Rule** para agregar una nueva regla en el Edge Gateway.
4. Para el **Name** ingrese **vCenter Inbound Rule**.
5. Haga click en **Set Source** para definir el origen de la regla de firewall.

### Seleccione el Origen de la regla de Firewall

![SDDC-Network-06](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc06.jpg)

1. Haga click en el botón al lado de **Any**.
2. Haga click en **Save** para guardar la información de origen en la regla.

### Configuración de regla de Firewall para acceso a vCenter (Continuación)

![SDDC-Network-07](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc07.jpg)

1. Haga click en **Set Destination** para iniciar una nueva ventana y establecer el destino de la regla.

### Seleccionar el destino de la regla de Firewall

![SDDC-Network-08](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc08.jpg)

1. Haga click en el botón de radio al lado de **System Defined Groups**.
2. Seleccione la casilla de verificación junto a **vCenter**.
3. Haga click en **Save** para guardar la información de destino en la regla.

### Configuración de regla de Firewall para acceso a vCenter (Continuación)

![SDDC-Network-09](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc09.jpg)

Continúe configurando la regla de entrada de vCenter:

1. Haga click en el cuadro abajo de **Services** y seleccione **HTTPS (TCP 443)** para permitir el acceso SSL al servidor vCenter.
2. Publique las reglas haciendo click en el botón **Publish** para activar la regla de firewall.

vCenter ahora debe ser accesible desde cualquier lugar en internet. En la siguiente sección, accederemos al cliente HTML5 de vCenter para configurar las máquinas virtuales.

## Iniciar sesión de vCenter en VMware Cloud on AWS

![SDDC-vcenter-010](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc010.jpg)

La configuración para conectarse al servidor vCenter asociado con el SDDC está disponible en la pestaña de configuración (Settings) para el SDDC. Vamos a conectarnos al servidor vCenter e iniciar sesión.

1. Haga click en la pestaña **Settings** en el SDDC que configuramos en la última lección.
2. Haga click en la **flecha** junto a vCenter User Account para exponer los detalles de inicio de sesión. En este laboratorio utilizaremos el usuario predeterminado cloudadmin@vmc.local.
3. Copie el **password** haciendo click en los dos cuadrados al lado de la contraseña. Esto lo copiará al portapapeles de las consolas.
4. Haga click en la **flecha** junto a **vSphere Client (HTML5)** para exponer la URL de vCenter.
5. Haga click en el enlace **URL** para **abrir** vSphere Client en otra pestaña.

**NOTA: Si experimenta algún problema de inicio de sesión a continuación, puede hacer click en los dos cuadros junto a la URL a continuación para pegar la URL en una ventana de incógnito. Esto no debería ser necesario normalmente.**

### Iniciar sesión en el vSphere Client (HTML5)

Para iniciar una sesión en el vSphere Client (HTML5)Ñ

![SDDC-vcenter-011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc011.jpg)

1. En el campo del nombre de usuario ingrese **cloudadmin@vmc.local**
2. Haga click con el botón derecho en el campo **Password** y pegue la contraseña copiada en el paso anterior.
3. Haga clic en **Login**.

### vSphere Client (HTML5)

![SDDC012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc012.jpg)

Ha iniciado sesión en su VMware Cloud on AWS vCenter Server como usuario cloudadmin@vmc.local.

## Creación de un Content Library

Las bibliotecas de contenido son contenedores para objetos como plantillas de VM, plantillas de vApp, y otros tipos de archivos como imágenes ISO.

Es posible crear bibliotecas de contenido usando vSphere Web Client, y agregarles plantillas, con las cuales es posible desplegar máquinas virtuales o vApps en su ambiente VMware Cloud on AWS si ya tiene una biblioteca de contenido en su centro de datos el contenido puede ser importado a su SDDC.

Puede crear dos tipos de bibliotecas de contenido: contenido local o suscrita.

### Bibliotecas Locales

Puede usar las bibliotecas locales para almacenar objetos en una sola instacia de vCenter Server. Puede publicar la biblioteca local para que usuarios en otros sistemas vCenter Server se suscriban a ella. Cuando se publica una biblioteca de contenido externamente, es posible configurar una contraseña como medio de autenticación.

Las plantillas de VM y las plantillas de vApp son almacenadas como archivos OVF en la biblioteca de contenido. También se pueden cargar otros tipos de archivos como imágenes ISO, archivos de texto y demás.

### Bibliotecas Suscritas

La suscripción a una biblioteca publicada se realiza al crear una biblioteca suscrita. Es posible crear una biblioteca suscrita en la misma instancia de vCenter Server donde la biblioteca esta publicada o en un vCenter Server diferente. En el asistente de creación de la biblioteca existe la opción para descargar todo el contenido de la biblioteca publicada inmediatamente luego de que la biblioteca suscrita haya sido creada o solamente descargar la metadata de los objetos de la biblioteca pubilcada y luego descargar el contenido de los objetos que se desean utilizar.

Para asegurar que el contenido de la biblioteca suscrita esté actualizado, la biblioteca suscrita se sincroniza automáticamente con la biblioteca publicada de origen en intervalos regulares. También es posible sincronizar la biblioteca manualmente.

Puede usar la opción de descarga de contenido de la biblioteca publicada inmediatamente o solamente cuando sea necesario para administrar el almacenamiento.

La sincronización de una biblioteca suscrita que tiene la opción de descargar todo el contenido de la biblioteca publicada inmediatamente, sincronizar tanto como la metadata de los objetos así como ellos. Durante la sincronización de la biblioteca los nuevos objetos son completamente descargados a la ubicación configurada en la biblioteca suscrita.

La sincronización en una biblioteca suscrita que tiene la opción de descargar el contenido solo cuando es necesario, sincronizar solamente la metadata de los objetos de la biblioteca publicada y no descargar los objetos. Esto ahorra espacio. Si un objeto de la biblioteca debe ser utilizado es necesario sincronizar el objeto. Luego de usarlo es posible eliminar el objeto para liberar espacio.

Si utiliza una biblioteca suscrita, solo puede utilizar el contenido, pero no contribuir con nuevos elementos. Solo el administrador de la biblioteca publicada puede administrar los archivos y plantillas.

### Acceso a Bibliotecas de Contenido en el vSphere Client

![SDDC013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc013.jpg)

1. Haga click en **Menu**
2. Haga clicn en **Content Libraries**

### Suscripción a una Biblioteca de Contenido existente

![SDDC014](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc014.jpg)

1. En la ventana Content Library, haga click en el símbolo **+** para agregar una Biblioteca de Contenido.

![SDDC015](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc015.jpg)

1. Escriba **VMC Content Library** para el nombre de su biblioteca.
2. Haga click en el botón **Next**.

![SDDC016](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc016.jpg)

1. Seleccione el botón de radio adjunto a **Subscribed content library**
2.. En el campo **Subscription URL** entre lo siguiente: https://vmc-elw-vms.s3-accelerate.amazonaws.com/lib.json
3. **No seleccione** la caja de verificación **Enable authentication**
4. Asegúrese que la opción **Download content** se encuentre en **immediately**
5. Haga click en **NEXT**

![SDDC017](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc017.jpg)

1. Elija **WorkloadDatastore** como la ubicación de almacenamiento
2. Haga click en el botón **Next**

![SDDC018](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc018.jpg)

1. Haga click en el botón *Finish*.

**Nota: Dependiendo del tamaño y la cantidad de plantillas, puede llevar un tiempo sincronizar el contenido. Esta biblioteca de contenido solo debería tomar unos minutos para sincronizar.**

## Creación de una Especificación de Personalización para Linux

Cuando se clona una máquina virtual o se despliega desde una plantilla, es posible personalizar el sistema operativo de la VM para cambiar propiedades como el nombre del red, la configuración de red o licenciamiento.

Personalizar los sistemas operativos de las VMs ayuda a prevenir conflictos que pueden resultar en VMs desplegadas con configuraciones idénticas como por ejemplo nombres de red iguales.

Se pueden especificar características personalizadas abriendo el asistente de Guest Customization durante los procesos de clonación o despliegue. Alternativamente, se pueden crear especificaciones de personalización, que son configuraciones de personalización almacenadas en la base de datos de vCenter. Durante los procesos de clonación o despliegue, es posible seleccionar una especificación de personalización para aplicar a esta nueva máquina virtual.

Use el Customization Specification Manager para administrar las especificaciones de personalización que son creadas usando el asistente de Guest Customization.

### Navegar al área de Customization Specifications

![SDDC019](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc019.jpg)

1. Haga click en **Menu**
2. Haga click en **Policies and Profiles**

### Agregar una Espeficación de Personalización para VMs

![SDDC020](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc020.jpg)

1. Haga click en **+ New** para agregar una nueva Especificación de Personalización para Linux

### Definición de detalles de la Especificación de Personalización

![SDDC021](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc021.jpg)

1. Especifique un nombre a la Especificación de Configuración de la VM
2. Escriba una descripción (Opcional)
3. Asegúrese de seleccionar **Linux** a la par de **Target guest OS**
4. Haga click en el botón **Next**

### Definición estándar de nomenclatura de la Especificación

![SDDC022](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc022.jpg)

1. Haga click en el botón a la par de **Use the virtual machine name**
2. Para **Domain name** escriba **corp.local**
3. Haga click en el botón **Next** para continuar

### Seleccionar zona horaria

![SDDC023](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc023.jpg)

1. Seleccione la zona horaria apropiada en el campo **Area**
2. Seleccione el **Location** apropiado
3. Haga click en el botón **Next** para continuar

### Seleccionar la configuración de red

![SDDC024](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc024.jpg)

1. Asegúrese que el botón a la par de **Use standard network settings for the guest operating system, including enabling DHCP in all network interfaces** esté seleccionado
2. Haga click en **Next** para continuar

### Entrar configuración de DNS

![SDDC025](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc025.jpg)

1. Escriba **8.8.8.8** como el servidor DNS primario.
2. Escriba **8.8.4.4** como el servidor DNS secundario.
3. Para las rutas de búsqueda DNS ingrese **corp.local**.
4. Haga click en el botón **Add** para agregar el dominio corp.local a la ruta de búsqueda DNS.
5. Haga click en **Next** para continuar.

### Finalizar la creación de la especificación

![SDDC026](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc026.jpg)

1. Revise su información y haga click en el botón de **Finish**.

### Especificación creada

![SDDC027](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc027.jpg)

Felicitaciones! Ha creado exitosamente una Especificación de Personalización para maquinas virtuales Linux. Tambien es posible Duplicar, Editar, Importar y Exportar una Especificación de Configuración para VM.

## Implementación de una Máquina Virtual

![SDDC-deploy-vm-013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc013.jpg)

En la ventana del vSphere Client ya abierta, implemente una plantilla desde la biblioteca de contenido:

1. Haga click en **Menu**
2. Haga click en **Content Libraries**

### Seleccionar Biblioteca de Contenido

![SDDC-deploy-vm-028](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc028.jpg)

1. Haga click en **VMC Content Library** que fue sincronizado anteriormente.

### Desplegar una nueva máquina virtual desde una plantilla

![SDDC-deploy-vm-029](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc029.jpg)

1. Haga click en la pestaña **Templates** para acceder a las plantillas sincronizadas en la biblioteca de contenido.
2. Haga click derecho en la plantilla **photoapp-u** para enseñar el menú de Actions.
3. Haga click en **New VM from this Template** para desplegar una máquina virtual desde esta plantilla.

### Escoger Nombre y Localización de la máquina virtual

![SDDC-deploy-vm-030](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc030.jpg)

1. Ingrese **webserver01** para el nombre de la máquina virtual.
2. Haga clic en la **flecha** junto a SDDC-Datacenter para exponer las carpetas disponibles.
3. En VMware Cloud on AWS, las cargas de trabajo del cliente deben ubicarse en la carpeta de Workloads (o subcarpeta). Haga clic en la carpeta **Workloads**.
4. Seleccione la casilla de verificación junto a **Customize the operating system**.
5. Haga clic en **Next** para continuar.

### Escoger Especificación de Personalización de la máquina virtual

![SDDC-deploy-vm-031](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc031.jpg)

Utilizaremos la especificación de personalización creada en el módulo anterior para personalizar el sistema operativo.

1. Haga click para seleccionar la especificación de personalización **LinuxSpec**.
2. Haga click en **Next** para continuar.

### Seleccionar el Resource Pool

![SDDC-deploy-vm-032](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc032.jpg)

1. Haga click en la flecha junto a **Cluster-1** para exponer los grupos de recursos disponibles.
2. En VMware Cloud on AWS, las cargas de trabajo de los clientes se deben colocar en **Compute-ResourcePool** (o subgrupo). Haga click en **Compute-ResourcePoo**.
3. Haga click en **Next** para continuar.

### Revisar Detalles de la Plantilla

![SDDC-deploy-vm-033](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc033.jpg)

Revise los detalles de la plantilla a desplegar. Es posible que se muestre una advertencia de seguridad, pero puede ignorar de forma segura para el propósito de este laboratorio.

1. Haga click en **Next** para continuar

### Seleccionar Almacenamiento

![SDDC-deploy-vm-034](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc034.jpg)

Cada VMware Cloud on AWS SDDC incluirá dos almacenes de datos para separar la administración y las cargas de trabajo de los clientes. Todas las cargas de trabajo de los clientes deben colocarse en el almacén de datos denominado WorkloadDatastore.

1. Haga click en **WorkloadDatastore** para seleccionar el almacén de datos donde se aprovisionará la máquina virtual.
2. Haga click en **Next** para continuar

### Seleccionar la red para la máquina virtual

![SDDC-deploy-vm-035](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc035.jpg)

Utilizaremos el segmento de red creado en un ejercicio previo para estas máquinas virtuales.

1. Haga click en la flecha debajo de **Destination Network** para seleccionar la red para la máquina virtual.
2. Haga click en **Demo-Net** para seleccionar la red creada anteriormente.
3. Haga click en **Next** para continuar.

### Completar el despliegue de la máquina virtual

![SDDC-deploy-vm-036](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc036.jpg)

1. Revise la información para verificar la precisión y haga click en **Finish** para implementar la máquina virtual.

La máquina virtual debería tardar un par de minutos en implementarse. Continúe con el siguiente ejercicio para clonar esta máquina virtual para crear un segundo servidor web.

## Clonar una máquina virtual

En este ejercicio, clonará la máquina virtual creada en el ejercicio anterior para crear un segundo servidor web.

### Navegar a máquinas virtuales y plantillas

![SDDC-clone-vm-037](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc037.jpg)

1. Valide la implementación de la máquina virtual completada en el ejercicio anterior buscando la tarea **Deploy OVF Template** y verificando que esté **Complete**.
2. Si está completo, haga click en ** Menú **.
3. Haga clic en **VMs and Templates** para navegar a la vista de VMs y plantillas.

### Seleccionar y encender webserver01

![SDDC-clone-vm-038](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc038.jpg)

Antes de poder clonar el servidor web, primero necesitaremos encender la máquina virtual para que la especificación de personalización pueda ejecutarse:

1. Haga click en la flecha junto a **SDDC-Datacenter** para exponer las subcarpetas.
2. Haga click en la flecha junto a las cargas de trabajo para exponer **webserver01**
3. Haga click en la máquina virtual **webserver01**
4. Haga click en la **flecha verde** en la parte superior central de la pantalla para ejecutar la operación de encendido.

**Nota: espere hasta que la máquina virtual esté completamente encendida antes de continuar con el siguiente paso.**

<aside class="notice">
<font color="dodgerblue">
<img src="https://s3-us-west-2.amazonaws.com/vmc-workshops-images/info.jpeg" width="25" height="25"> Si el servidor web no se conecta a la red y no recibe una dirección IP de DHCP, asegúrese de que la NIC esté conectada haciendo click con el botón derecho en <b>webserver01</b> y luego <b>Edit Settings</b> y asegúrese de que la casilla de verificación junto a Connected esté seleccionada. Es posible que deba repetir este paso para la VM clonada webserver02.
</font>
</aside>

### Iniciar clonación de la máquina virtual

![SDDC-clone-vm-039](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc039.jpg)

Ahora comenzaremos el proceso de clonación de esta máquina virtual.

1. Haga click con el botón derecho en **webserver01** para exponer el menú Actions.
2. Haga click en **Clone** para exponer un menú secundario de opciones.
3. Haga click en **Clone to Virtual Machine** para iniciar el asistente de clonación.

### Seleccione el nombre y la carpeta de la máquina virtual

![SDDC-clone-vm-040](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc040.jpg)

1. Junto a **Virtual machine name** ingrese **webserver02**.
2. Haga click en la carpeta **Workloads** para la ubicación de la máquina virtual.
3. Haga clic en **Next** para continuar.

### Seleccionar recurso de cómputo de la máquina virtual

![SDDC-clone-vm-041](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc041.jpg)

1. Haga click en **Compute-ResourcePool** para asegurarse de que esté seleccionado para la máquina virtual.
2. Haga click en **Next** para continuar.

### Seleccionar el datastore de la máquina virtual

![SDDC-clone-vm-042](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc042.jpg)

1. Haga click en **WorkloadDatastore** para asegurarse de que esté seleccionado como destino para la máquina virtual.
2. Haga click en **Next** para continuar.

### Seleccionar opciones de clonación

![SDDC-clone-vm-043](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc043.jpg)

Ahora vamos a configurar las opciones para la clonación. Tendremos que personalizar el sistema operativo para cambiar el nombre del servidor y también encender la máquina virtual después de que se complete la clonación.

1. Haga click en la casilla de verificación junto a **Customize the operating system**.
2. Haga click en la casilla de verificación junto a **Power on virtual machine after creation**.
3. Haga click en **Next** para continuar.

### Elija la especificación de personalización de la máquina virtual

![SDDC-clone-vm-044](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc044.jpg)

Utilizaremos la especificación de personalización creada en un ejercicio anterior para personalizar el sistema operativo.

1. Haga click para seleccionar la especificación de personalización **LinuxSpec**.
2. Haga click en **Next** para continuar.

### Completar la implementación de la máquina virtual

![SDDC-clone-vm-045](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc045.jpg)

1. Revise la información para verificar la precisión y haga click en **Finish** para clonar la máquina virtual.

Debería tardar un par de minutos en clonar la máquina virtual. Continúe con el próximo ejercicio para obtener información sobre cómo proteger las cargas de trabajo en VMware Cloud on AWS.

<aside class="notice">
<font color="dodgerblue">
<img src="https://s3-us-west-2.amazonaws.com/vmc-workshops-images/info.jpeg" width="25" height="25"> If the webserver doesn't connect to the network and does not receive and IP address from DHCP, ensure the NIC is connected by right-clicking on <b>webserver01</b> and then <b>Edit Settings</b> and make sure the checkbox next to Connected is selected.  You may need to repeat this step for the cloned VM webserver02.
</font>
</aside>

## Probando la conectividad entre las máquinas virtuales

En este ejercicio probaremos la conectividad entre webserver01 y webserver02, que creamos en los ejercicios anteriores.

### Abrir la consola de webserver01

![SDDC-test-vm-046](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc046.jpg)

Necesitamos abrir una sesión de consola para webserver01 para validarla y poder comunicarnos con webserver02.

1. En vSphere Client (HTML5), haga click en webserver01 para enfocarlo.
2. Haga click en el cuadro negro debajo de Summary en el centro de la pantalla. Esto intentará iniciar una sesión de consola, pero puede fallar porque se bloqueó la ventana emergente. Si esto ocurre, siga los pasos 3 a 6, de lo contrario, continúe con la siguiente sección.
3. Haga click en el ícono con la pequeña x roja en la barra de direcciones de Chrome para iniciar el cuadro de diálogo del bloqueador de elementos emergentes.
4. Haga click en el botón de radio junto a Always allow los pop-ups desde https://vcenter.sddc-xx-xx-xxxx.vmwarevmc.com
5. Haga click en el botón Done.
6. Regrese al cuadro negro debajo del Summary y haga click nuevamente. La sesión de consola debe iniciarse en una nueva pestaña.

### Encuentre la dirección IP de webserver02

![SDDC-test-vm-047](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc047.jpg)

Antes de que podamos probar la conectividad entre los dos servidores, necesitamos encontrar la dirección IP de webserver02.

1. Haga click en la pestaña de Chrome de vSphere Client (HTML5) para volver a enfocar.
2. Haga click en la máquina virtual **webserver02**.
3. Tome nota de la **Dirección IP** para webserver02 en el centro de la pantalla. Esto será necesario en el siguiente paso.
4. Haga click en la la pestaña de la sesión de la consola para webserver01 para volver a enfocarla.

### Iniciar sesión y hacer un ping a webserver02

![SDDC-test-vm-048](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc048.jpg)

Ahora que tenemos la dirección IP para webserver02, configuremos un ping continuo al servidor para verificar la comunicación.

Antes de comenzar, haga click en cualquier lugar dentro de la ventana de la consola para enfocarla.

1. En el indicador de inicio de sesión, ingrese **root** y presione enter.
2. Cuando se le solicite la contraseña, ingrese **VMware1!** y presione enter.
3. Cuando se solicite la consola, ingrese **ping 10.10.xx.xxx** y presione enter. El tercer octeto se basa en el número de estudiante y el último octeto de la dirección IP en la mayoría de los casos será 101, pero verifique esto en su configuración.
4. Verifique que los pings sean exitosos.

**NOTA: Deje esta ventana de ping y consola abierta para la siguiente lección. Lo volveremos a visitar para verificar que los servidores web ya no puedan comunicarse.**

¡Felicidades! Ahora ha implementado dos servidores web en VMware Cloud on AWS SDDC y ha verificado que pueden comunicarse entre sí. En la siguiente lección, crearemos reglas de firewall para impedir que los servidores se comuniquen entre sí y también hacer que webserver02 sea accesible desde Internet.

## Configurar Servicios Avanzados de red en VMware Cloud on AWS

Servicios Avanzados de red en VMware Cloud on AWS están ahora disponibles en despliegues nuevos de SDDC.

### Firewall Distribuido en Servicios Avanzados de red en VMware Cloud on AWS

![DFW-01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw01.jpg)

Al usar VMware Cloud on AWS Advanced Network Services, los usuarios tienen la capacidad de implementar microsegmentación con Firewall distribuido. Las políticas de seguridad granular se pueden aplicar a nivel de VM, lo que permite la segmentación dentro de la misma red L2 o en redes L3 independientes. Esto se muestra en el diagrama de arriba.

Toda la configuración de redes y seguridad ahora se realiza a través de la consola de VMware Cloud on AWS a través de la pestaña Redes y seguridad (Networking & Security), incluso la creación de segmentos de red. Esto facilita la operación y la administración al tener acceso a todas las redes y la seguridad a través de la consola.

### Firewall Distribuida

![DFW-02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw02.jpg)

En la captura de pantalla arriba, puede ver, además de la capacidad de crear múltiples secciones, los usuarios pueden organizar las reglas del Firewall distribuido en grupos (Reglas de emergencia, Reglas de infraestructura, Reglas de entorno y Reglas de aplicación. Las reglas se tocan desde arriba hacia abajo.

### Grupos de Seguridad

![DFW-03](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw03.jpg)

Además de las nuevas capacidades de Firewall distribuido, ahora se pueden aprovechar los objetos de agrupación dentro de las políticas de seguridad. Los grupos de seguridad admiten los siguientes criterios/construcciones de agrupación:

* Dirección IP
* Instancia VM
* Criterios de coincidencia del nombre de la máquina virtual
* Criterios de coincidencia de la etiqueta de seguridad

Como se muestra arriba, los grupos de seguridad se pueden crear bajo Grupos de carga de trabajo o Grupos de administración. Los Grupos de carga de trabajo se pueden usar en las políticas de firewall de DFW y CGW y los Grupos de administración se pueden usar en las políticas de firewall de MGW. Los grupos de administración solo admiten direcciones IP, ya que estos grupos se basan en la infraestructura. Ya existen grupos de administración predefinidos para vCenter, hosts ESXi y NSX Manager. Los usuarios también pueden crear grupos aquí según la dirección IP para los hosts ESXi locales, vCenter y otros dispositivos de administración.

### Ver VM's en un Grupo de Seguridad

![DFW-04](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw04.jpg)

Aquí puede ver que hemos implementado algunas máquinas virtuales en vCenter y puede ver las máquinas virtuales en el inventario dentro de la consola. Además, hemos etiquetado las máquinas virtuales con etiquetas de seguridad de Web, aplicaciones y bases de datos, respectivamente.

### Etiquetando máquinas virtuales

![DFW-tag-05](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw05.jpg)

A través de la consola de VMware Cloud on AWS podemos aplicar etiquetas de seguridad a máquinas virtuales para después agruparlas.

Regrese de nuevo a la consola de VMware Cloud on AWS.

1. Haga click en la pestaña de VMware Cloud on AWS en Chrome e inicie una sesión con la información proveída en caso que su sesión haya expirado.
2. Haga click en **View Details** para acceder a los detalles del SDDC.

### Editar etiqueta para webserver01

Ahora empezaremos etiquetando las máquinas virtuales con etiquetas de seguridad.

![DFW-tag-06](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw06.jpg)

1. Haga click en la pestaña de **Networking & Security** para acceder a la configuración de redes.
2. En el panel izquierdo, haga click en **Groups**.
3. Bajo Groups, haga click en **Virtual Machines** para acceder a las máquinas virtuales que son parte del SDDC.
4. Localice **webserver01** y haga click en los 3 puntos verticales y click en **Edit**.

### Agregar etiqueta de seguridad para Web

![DFW-tag-07](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw07.jpg)

1. Bajo Tags, escriba **Web** para webserver01.
2. Haga click en **Save** para hacer los cambios.

### Editar etiqueta para webserver02

![DFW-tag-08](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw08.jpg)

Ahora etiquetaremos webserver02 con la misma etiqueta Web. Usaremos esta etiqueta Web para crear un grupo para los dos servidores web.

1. Localice **webserver02** y haga click en los 3 puntos verticales y click en **Edit**.

### Agregar etiqueta de seguridad a webserver02

![DFW-tag-09](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw09.jpg)

1. Bajo Tags, entre **Web** para webserver02.
2. Haga click en **Save** para guardar los cambios.

### Creación de un grupo dinámico

![DFW-tag-010](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw010.jpg)

Los grupos se pueden usar en VMware Cloud on AWS Advanced Network Services para agrupar máquinas virtuales y simplificar la configuración de la base de reglas. En este ejercicio agruparemos los dos servidores web en un grupo y luego crearemos una regla de firewall para bloquear la comunicación entre ellos. En una aplicación tradicional correctamente diseñada, generalmente no hay necesidad de que los servidores en el nivel web se comuniquen.

Ahora crearemos un grupo de servidores web basado en la etiqueta de seguridad dinámica que aplicamos anteriormente.

1. Haga click en **Workload Groups**
2. Haga click en **Add Group**
3. Bajo nombre escriba **Web** para nombre del grupo
4. Bajo Member Type, haga click en el menú desplegable y selecione **Membership Criteria**
5. Bajo Members haga click en **Set Membership Criteria**

### Agregar criteria de afiliación

![DFW-tag-011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw011.jpg)

Ahora agregaremos la criteria para agrupar máquinas basado en la etiqueta de seguridad.

1. Haga clic en **+ Add Criteria**
2. Bajo Property, haga click en el menú desplegable y seleccione **Tag**
3. Bajo Value, entre **Web**
4. Haga click en **Save** para continuar

### Guardar cambios a grupo de cargas

![DFW-tag-012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw012.jpg)

1. Haga click en **Save** para guardar los cambios

### Ver Miembros

![DFW-tag-013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw013.jpg)

Ahora podemos validar la afiliación al grupo funcione como esperamos.

1. Haga click en los 3 puntos verticales a la part del grupo Web.
2. Haga click en **View Members** para mostrar los miembros del grupo dinámico.

### Validar Miembros de Grupo

![DFW-tag-014](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw014.jpg)

1. Valide que tanto **webserver01** como **webserver02** aparezcan en el grupo como miembros. Si no aparecen, regrese y asegúrese que no haya cometido un error.
2. Haga click en **Close**.

Ahora que este grupo está creado, usted puede fácilmente agregar nuevos miembros con solo aplicar una etiqueta de seguridad.

### Crear una sección de regla de Firewall

![DFW-tag-015](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw015.jpg)

Ahora que hemos creado un grupo dinámico, crearemos una regla de Firewall para bloquear acceso entre los servidores web.

1. Haga click en **Distributed Firewall** en el panel izquierdo de su pantalla.
2. Haga click en **Application Rules**.
3. Haga click en **Add New Section** para crear una sección nueva para la regla. Esta funcionalidad nos permite agrupar reglas lógicamente para poder operar el ambiente de una manera más fácil.
4. Bajo Name, escriba **Web Tier**.
5. Haga click en **Publish** para hacer los cambios.

### Agregar regla de Firewall

![DFW-tag-016](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw016.jpg)

Ahora que tenemos esta sección creada, podemos agregar una regla de Firewall.

1. Haga click en la flecha a la par de la sección **Web Tier**.
2. Haga click en **Add New Rule** en el menú arriba de las reglas.
3. Bajo Name, escriba **Block Web To Web**.
4. Bajo Action, haga click en la flecha y seleccione **Drop**.
5. Bajo Sources, haga click en **Any**.

### Seleccionar Origen

![DFW-tag-017](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw017.jpg)

1. Haga click en el menú deplegable a la par de Web.
2. Haga click en **Save** para guardar los cambios a la regla.

### Agregar Destino

![DFW-tag-018](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw018.jpg)

1. Bajo Destination haga click en **Any**.

### Seleccione Destino

![DFW-tag-019](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw019.jpg)

1. Haga click en la caja a la par de Web.
2. Haga click en **Save** para guardar los cambios a la regla.

### Publicar Regla de Firewall

![DFW-tag-020](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw020.jpg)

1. Haga click en **Publish** para guardar la regla y bloquear el tráfico entre los servidores web.

### Probar la Regla de Firewall Distribuida

![DFW-tag-021](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw021.jpg)

Todavía debería tener la sesión de su consola abierta del ejercicio anterior al webserver01 y debe estar corriendo un comando de ping.

1. Haga click en la pestaña de Chomre de **webserver01**.
2. Haga un ping a la IP de webserver02 10.10.xx.xxx.

Los pings deben de haberse detenido lo cual significa que la regla de Firewall Disribuida se han aplicado correctamente. Esta sencilla demonstración le debe de dar una idea del poder del Firewall Distribuido.

## Conclusión 

En este módulo, exploramos la configuración de un SDDC de VMware Cloud on AWS inluyendo la utilización de una biblioteca de contenido, desplegación de máquina virtuales, modificación de reglas de Firewall y operaciones con máquinas virtuales.

## SDDC con un nodo individual

Se ha disfrutado estos ejercicios de éste módulo y le gustaría continuar experimentando con VMware Cloud on AWS, haga un scan del código QR abajo para empezar su experience con un SDDC de 1 nodo.

![DFW-tag-022](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw022.jpg)

Ha completado este módulo.

Favor agregue comentarios abajo si le gustaría compartir su opinión sobre el contenido de estos ejercicios. Gracias.