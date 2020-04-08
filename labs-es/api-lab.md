---
layout: single
title: "VMware Cloud on AWS API Lab Manual"
date: 2018-07-18
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---
# Introducción

En este ejercicio mostraremos cómo puede interactuar con VMware Cloud on AWS a través de medios programáticos. Veremos cómo podemos usar PowerShell como un medio para interactuar con la plataforma de soluciones de la nube, así como con la instancia de vCenter. Después, profundizaremos en cómo podemos interactuar con el API REST de VMware Cloud on AWS y realizar acciones tanto en la pestaña integrada de "Developer Center" en la consola, como a través de populares clientes REST de código abierto y de otras herramientas. Continuaremos nuestro ejercicio haciendo uso de "Postman" como nuestro cliente REST.

## Usando PowerShell

![APIs1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs1.jpg)

1. Haga click en **Start**, y baje hasta que vea el menú Windows PowerShell
2. Haga click derecho en el acceso directo de **PowerShell** CLI y seleccione **Run as Administrator**

    ![APIs3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs3.jpg)

Instale el módulo VMware PowerCLI

 ```powershell
Install-Module VMware.PowerCLI
```

**NOTA:** Se le preguntará si desea instalar el NuGet provider, elija la opción por defecto o escriba **Y** y luego intro, luego se le preguntará si desea confiar en un repositorio no confiado, **NO ELIJA** la opción por defecto y escriba **Y** y luego intro.

![APIs4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs4.jpg)

Ahora es necesario cambiar la politica de ejecución a Remote Signed.

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Force
```

![APIs5](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs5.jpg)

Ahora deberá configurar el PowerCLI Configuration a Ignore Invalid Certificates.

**PASO IMPORTANTE:**

```powershell
Set-PowerCLIConfiguration -InvalidCertificateAction Ignore -Confirm:$false -WarningAction:SilentlyContinue
```

**NOTA:** Asegúrese de que la "i" en "Ignore" sea mayúscula

![APIs6](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs6.jpg)

Ahora es necesario instalar los comandos de VMware CLI

```powershell
Install-Module -name VMware.VMC -scope AllUsers -Force
```

![APIs7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs7.jpg)

Hagamos una revisión rápida de los comandos de VMware CLI

```powershell
Get-VMCCommand -WarningAction:SilentlyContinue
```

![APIs8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs8.jpg)

Ahora necesitará obtener el Refresh Token desde la consola de VMC. Cambie al browser o abra uno e ingrese a **vmc.vmware.com**.

Si todavía no ha ingresado

1. Abra una nueva pestaña
2. Haga click en el acceso directo VMware Cloud on AWS
3. Escriba su correo electrónico
4. Haga click en **Next**

    ![APIs9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs9.jpg)

5. Haga click en la caja desplegable junto a **Name/Org ID**
6. Haga click en **My Account**

    ![APIs10](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs010.jpg)

Ahora cree un nuevo refresh token para su ID vinculada a esta Org

1. Haga click en **API Tokens**
2. Haga click en **Generate a New API Token**

    ![APIs011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs011.jpg)

1. Asigne un nombre al token
2. Seleccione la casilla de **Organization Owner**
3. Seleccione la casilla de **VMware Cloud on AWS**
4. Haga click en el botón **Generate**

    ![APIs012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs012.jpg)

5. Haga click en el botón **Copy** para guardar el token en su guardapapeles.

***NOTA*** Asegurese de guardar este refresh token en un lugar seguro para poder usarlo de nuevo en la siguiente sección usando los API's en Postman.

Ahora conectemos el servidor de VMC, ingrese el comando a continuación y agreguelo al refresh token despues del parametro -refreshtoken

```powershell
connect-vmc -refreshtoken
```

![APIs13](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs13.jpg)

Ahora que estamos conectados a nuestra organizacion de VMC a traves de PowerShell, podemos observar las organizaciones a las que tenemos acceso usando el siguiente comando:

```powershell
Get-VMCorg
```

Tome nota de el Org Display_Name y ID

![APIs14](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs14.jpg)

Ahora que sabemos el Org Display_Name podemos buscar información sobre el SDDC dentro de nuestra org.

**NOTA**: Reemplace el # con su numero asignado de estudiante

```powershell
Get-VMCSDDC -Org VMC-WS#
```

![APIs15](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs15.png)

Otra cosa interesante que se puede hacer es ver las Credenciales por Defecto de su SDDC

```powershell
Get-VMCSDDCDefaultCredential -org VMC-WS#
```

**NOTA:** reemplace # con su número de estación de trabajo.

## REST APIs usando Developer Center

En este módulo se usarán REST APIs para obtener información básica sobre la organización y el SDDC. Para hacer esto se usará el nuevo Developer Center, que fue creado para el uso de APIs y scripts con los que se puede crear, agregar y eliminar SDDCs, además de poder conectarse y usar todas las APIs de vCenter. Para iniciar, regrese al ambiente VMC.

![DeveloperCenter1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter1.jpg)

Inicie el navegador Chrome en su escritorio virtual asignado

![DeveloperCenter2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter2.jpg)

Si todavia no ha ingresado, ingrese a su organizacion de VMware Cloud on AWS.

1\. Desde dentro de la pestaña VMware Cloud on AWS, haga click en el menu Developer Center

    ![DeveloperCenter3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter3.jpg)

En el Developer Center hay muchos recursos para explorar. Por ejemplo, revisemos código de ejemplo que fue cargado por uno de los programadores de nuestra API. Si recorre esta pantalla verá diferentes ejemplos de código para Postman (un Ambiente de Desarrollo para REST API )

Tambien se encuentran ejemplos para Python, PowerCLI, y muchos otros. Cualquiera puede contribuir con ejemplos de código para la comunidad, si tiene interés visite http://code.vmware.com ó haga click en el enlace **VMware{code} Sample Exchange**.
2. Haga click en *Code Samples* en el menú
3. Haga click en *Download* en el cuadro "PowerCLI - VMC Example Scripts"

    ![DeveloperCenter4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter4.jpg)

Luego de que el script se descargue
4. Haga click en la flecha desplegable
5. Haga click en **Show in Folder**
6. Haga unzip al archivo **PowerCLI-Example-Scripts-master.zip**
7. Abra la carpeta **PowerCLI-Example-Scripts-master**
8. Abra la carpeta **Scripts**
9. Abra la carpeta **VMware_Cloud_on_AWS**
10. Haga click derecho en el script **VMC Example Script.ps1**
11. Haga click en Edit

  Esto abrirá el ambiente de PowerShell ISE. Ahora podrá ver los comandos de PowerShell que usó en le módulo anterior así como otros comandos que puede usar con su SDDC. Cierre la ventana de PowerShell ISE.

    ![DeveloperCenter6](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter6.jpg)

  Vamos ahora a ejecutar unos comandos simples de REST API integrados a Developer Center, regrese a su navegador

12. Haga click en el menú de API Explorer
13. Asegurese de seleccionar su SDDC
14. Haga click en la flecha desplegable al lado de Organization
15. Haga click en la flecha desplegable al lado del primer "GET" API
16. Haga click en *Execute*

    ![DeveloperCenter7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter7.jpg)

  Que es lo que no hicimos? No pusimos ningún tipo de autenticación para obtener estos datos. La razón es porque estamos utilizando una sesión de autenticación para ejecutar estos comandos. Para correr estos comandos en otras herramientas como PowerShell o Postman, necesitaríamos obtner nuestro recurso y token de sesión antes de poder correr estos comandos.

  Examinemos la respuesta.
17. Aquí observamos el nombre alfanumérico de nuestra Organización, el cual también puede encontrar en *\#3*
18. El *ID* de la organización *NOTA*: Copie el número de ID, sin comillas, para poder utilizarlo en el siguiente paso
19. El *Display_Name* de la organización
20. La versión de la organización

    ![DeveloperCenter8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter8.jpg)

En este paso, se obtendrá información usando el metodo GET acerca de su organización
21. Haga click en la flecha desplegable cerca a los SDDCs
22. Haga click en *GET*
23. El Org ID debería estar completado, otra característica incluida por los programadores basada en la retroalimentación de los clientes. *NOTA:* Si el Org ID no se completa automáticamente, péguelo.
24. Haga click en **Execute**

    ![DeveloperCenter9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter9.jpg)

Ahora revisemos el cuerpo de la respuesta
25. La fecha de creación del SDDC
26. La SDDC ID
27. El estado del SDDC

## Postman

En este módulo, exploraremos como usar Postman para hacer invocaciones REST API y construir automatizaciones usando colecciones. Postman es una herramienta para el desarrollo de APIs. Por ejemplo, se pueden crear variables para usar dentro de APIs, probar respuestas y usar webhooks para integrarse con plataformas de colaboración.

![Postman1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman1.jpg)

Postman es facil de instalar. Para instalar Postman.

1. Abra una nueva pestaña de browser y visite https://www.getpostman.com
2. Haga click en *Download the App*

    ![Postman2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman2.jpg)

3. Busque Postman para **Windows (64-bit)**, haga click en **Download**, haga doble click en el archivo que fue descargado, la instalación se completará sin interacción.

*NOTA:* Cierre las pestañas de Postman en Chrome

    ![Postman3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman3.jpg)

4. Haga click en el texto: *Skip Signing in and Take me straight to the app*

    ![Postman4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman4.jpg)

5. Desmarque *Show this window on launch*
6. Cierre esta ventana

    ![Postman5](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman5.jpg)

Regrese al browser, si no tiene una pestaña abierta para VMware Cloud on AWS, siga las instrucciones siguientes

7. Navegue hacia <https://github.com/vmware/vsphere-automation-sdk-rest/archive/master.zip> para descargar el vSphere Automation REST SDK.

  Nuestro equipo interno de desarrollo de API ha creado un SDK para los lenguajes más populares de la actualidad. Para este módulo, se usará el SDK para REST para mostrar que es muy fácil importar y reusar algunas colecciones pre creadas para crear las nuestras.

    ![Postman7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman7.jpg)

8. *Este paso dejado en blanco intencionalmente*
9. *Este paso dejado en blanco intencionalmente*
10. Haga click en le menú de download
11. Haga click en *Open*

    ![Postman8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman8.jpg)

12. Haga click en *Extract*
13. Haga click en *Extract all*

    ![Postman9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman9.jpg)

  Mantendremos la ruta del archivo por defecto

14. Desmarque la selección
15. Haga click en *Extract*

  Cierre la ventana de explorador de archivos

      ![Postman10](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman10.jpg)

Ahora que tenemos Postman instalado y el repositorio de github en nuestro sistema local, vamos a importar la colección VMC y usar algunas peticiones para construir nuestra propia colección.

16. Haga click en *Import*
17. Haga click en *Choose Files*

    ![Postman11](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman11.jpg)

Para importar el archivo json de la colección de VMC que acabamos de descargar.

18. Recorra el directorio donde se hizo la descarga. El directorio debería ser *C:\downloads\vsphere-automation-sdk-rest-master\vsphere-automation-sdk-rest-master\samples\postman*
19. Haga click en *VMware Cloud on AWS APIs.postman_collection.json*
20. Haga click en *Open*

    ![Postman12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman12.jpg)

Ahora es necesario obtener el refresh token para nuestra Org en VMC. Regrese a la pestaña de VMware Cloud en el browser

21. Haga click en la caja desplegable al lado de *Student Name/Org ID*
22. Haga click en *My Account*

  Ahora crearemos un nuevo Regresh Token para el ID asociado con esta Org.

  ***NOTA***: Si usted ya genero el token, use el mismo token que ya fue generado. Tambien puede volver a regenerar un nuevo token si es necesario.

    ![Postman13](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs010.jpg)

23. Haga click en la pestaña **API Tokens**
24. Haga click en **Generate a New API Token**

    ![APIs011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs011.jpg)

25. Dele un nombre al token.
26. Seleccione el checkbox **Organization Owner.**
27. Seleccione el checkbox **VMware Cloud on AWS.**
28. Haga click el botón **Generate**.

    ![APIs012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs012.jpg)

29. Haga click en el botón **Copy** para guardar el token en el portapapeles

    ![Postman16](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman16.jpg)

Regrese a Postman. Ahora es necesario configurar un ambiente en Postman para usarlo con VMC. Un ambiente es donde se crearán y almanecerán las variables. Estas variables pueden ser locales o globales, dependiendo del uso que se les de en Postman. En este módulo, solo se usarán variables locales.

30. Haga click en **New**
31. Haga click en **Environment**

    ![Postman17](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman17.jpg)

32. Dele el nombre de **VMC** al ambiente
33. En la columna Key escriba **refresh_token**
34. En la columba Value column use CTRL-V para pegar el refresh token que fue copiado en el paso anterior.
35. Haga click en **Add**
36. Cierre la ventana

    ![Postman18](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman18.jpg)

Ahora configure este ambiente como por defecto.

*NOTA:* Si no lo configura el ambiente *VMC* como el por defecto, las variables generadas no serán accesibles.

37. Haga click en el menú desplegable
38. Seleccione **VMC**

    ![Postman19](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman19.jpg)

Ahora se comenzará a construir la colección usando una petición de las incluidas en el SDK que fue importada anteriormente.

39. Haga click en **Collections**
40. Haga click en - **Authentication and Login**
41. Vea como esta petición tiene la variable refresh token que se definió en el paso anterior.

    *NOTA:* Si el ambiente no fue configurado con VMC, este paso fallará porque la variable refresh_token no estará definida.

42. Haga click en **Send**
43. Ahora será accesible el token que fué generado con refresh token. Este es el cuerpo o payload de la respuesta de la petición.

    ![Postman20](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman20.jpg)

44. Haga click en el ícono con el ojo

    Se verá que el token de acceso ha sido almacenado en la variable para invocaciones futuras. ¿Cómo se hizo esto? Se ejecuto una prueba en el cuerpo de la respuesta. Se verá en el próximo paso.

    ![Postman21](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman21.jpg)

45. Haga click en **Tests**

La variable access_token fue cargada usando javascript en el cuerpo de la respuesta. También se usa la función setEnvironmentVariable de Postman para crearla.

    ![Postman22](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman22.jpg)

Ahora se salva la petición en la colección para poder usarla luego.

46. Haga Click en el menú desplegable
47. Haga Click en **Save As**

    ![Postman23](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman23.jpg)

48. Cambie Request name por *Authorize*
49. Cambie Request description por *Get Access Token*
50. Haga click en **Create Collection**
51. Escriba **Workshop** y haga click en la caja de verificación *check box*

    ![Postman24](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman24.jpg)

52. Seleccione la carpeta **Workshop**
53. Haga click en **Save to Workshop**

    ![Postman25](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman25.jpg)

Una nueva ventana se desplegará indicando que se ha creado una nueva colección. No se hará nada más acá.

54. Cierre la ventana

    ![Postman26](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman26.jpg)

Crearemos algunos detalles de nuestra Org para enviarlos a Slack.

55. Haga click en **Orgs** y **List Orgs**
56. Haga click en **Headers**
57. Haga click **Send**
58. Ahora se verá aquí como se hace uso de la variable **access_token** como **csp-auth-token**. Esto autorizará la petición. *NOTA:* Este token de acceso tiene una duración de 30 minutos. Si al ejecutar la petición y aparece un mensaje **400 unauthorized**, regrese y ejecute la petición de autorización.
59. Revise el cuerpo de respuesta y encuentre el **display_name** de la Org

    ![Postman27](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman27.jpg)

Guarde este petición en nuestra colección y poder usarla luego.

60. Haga click en el menú desplegable
61. Haga click en **Save As**

    ![Postman28](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman28.jpg)

62. Cambie Request name a **Org list**
63. Cambie Request description a **Get a list of your Orgs**
64. Asegúrese que *Workshop* esté seleccionado en **Select a collection or folder to save to:**
65. Haga click en **Save to Workshop**

    ![Postman29](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman29.jpg)

Es necesario reemplazar el código de prueba que viene con el SDK para poder crear las variables que sean necesarias para usar el mensaje a Slack.

66. Haga click en **Tests**
    Copie y pegue el siguiente código en la sección **Tests**. *NOTA:* Es posible que tenga que oprimir CTRL-V para pegar el texto.
67. Haga click en **Send**

    ```javascript
    var jsonData = JSON.parse(responseBody);

    if (responseCode.code === 200) {
    for (i = 0; i < jsonData.length; i++) {
      pm.environment.set("name", jsonData[i].display_name);
      pm.environment.set("ID", jsonData[i].id);
      pm.environment.set("version", jsonData[i].version);
      pm.environment.set("state", jsonData[i].project_state);
       }
     }
     ```

    ![Postman30](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman30.jpg)

    Ahora es posible verificar si las variables han sido creadas y sus valores asignados.

68. Haga click en el ícono con el ojo
69.  Baje hasta encontrar las nuevas variables creadas.
      Una vez verificadas haga click en el ícono con el ojo de nuevo para cerrar la ventana

    ![Postman31](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman31.jpg)

    Guardar los cambios hechos a esta solicitud.
70. Haga click en **Save**

    ![Postman32](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman32.jpg)

    Ahora que se tienen los detalles de la Org, se enviará un mensaje a Slack.

    Para enviar mensajes a Slack un enlace debe ser generado para el canal de Slack en que queremos publicar. Esta ya ha sido realizado y esta en la parte inferior. Uno de los instructores tendrá el canal de Slack en uno de los monitores. Para que puedan ver los resultados.

Slack channel URL: 

    ```link
    https://hooks.slack.com/services/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
    ```

Ahora es necesario configurar la petición:

71. Haga click en el signo **+** para crear una nueva petición
72. Cambie el request type a **POST**
73. Corte y pegue la información del canal de Slack en el campo *address*
74. Seleccione **Body**
75. Cambie el format type a **raw**
76. Escriba el código que esta a continuación, o corte y péguelo en la sección Body. *NOTA:* Es posible que sea necesario oprimir CTRL-V para pegar el texto.

    ```json
    {
      "text" : "{% raw %}Your Org ID is: {{ID}}\nYour Org version is: {{version}}\nAnd your Org state is: {{state}}{% endraw %}",
      "username" : "{% raw %}{{name}}{% endraw %}"
    }
    ```

77. Haga click en **Send**

    ![Postman33](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman33.jpg)

    Salve la petición en nuestra colección para usarla luego.

78. Haga click en el menú desplegable
79. Haga click en **Save As**

     ![Postman34](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman34.jpg)

80. Cambie Request name a **Post to Slack**
81.  Cambie Request description a **Post some Org details to slack** Asegúrese de que Workshop esté seleccionado en *Select a collection or folder to save to:*
82. Click on **Save to Workshop**

Revise si la petición publicó Nombre, ID, Version, y Status de la Org.

    ![Postman35](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman35.jpg)

Ahora se mostrará como automatizar tareas usando una colección de Postman. Hasta ahora se han construido colecciones. Como se ve en la imagen hay 3 tareas en la colección Workshop.

83. Haga click en la flecha en la ventana Workshop
84. Click on **Run**

    ![Postman36](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman36.jpg)

85. Haga click en **Run Workshop**
86. Asegúrese de que **Environment** esté configurado a VMC

    ![Postman37](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman37.jpg)

    Si todo fue salvado y es ejecutado individualmente, debería correr exitosamente también.

87. Verifique el estado de cada petición.

Si el resultado es todo "200 OK" entonces podra observar otro envío en Slack para su Org del workshop.

Favor agregue sus comentarios en la parte de abajo si le gustaría agregar algún comentario o sugerencia acerca de este taller.