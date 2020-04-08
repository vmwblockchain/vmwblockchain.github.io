---
layout: single
title: "VMware Cloud on AWS API "
date: 2019-05-31
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---
# Introdução

Neste exercício de laboratório, mostraremos como você pode interagir com a plataforma VMware Cloud on AWS por meios programáticos. Vamos analisar como podemos usar o PowerShell como meio de interagir com o Cloud Solution Platform e com o vCenter. Em seguida, analisaremos como podemos interagir com a VMware Cloud através de API REST da AWS e executar ações no Developer Center intergrado na console e também por meio de REST. Para o propósito de nosso exercício de laboratório, estaremos fazendo uso de "Postman" como nosso cliente REST.

## Usando PowerShell

![APIs1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs1.jpg)

1. Clique em **Start**, e role para baixo até ver o menu do Windows PowerShell
2. Botão direito no ícone de atalho do **PowerShell** e selecione **Run as Administrator**

    ![APIs3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs3.jpg)

Instale o módulo VMware PowerCLI

```powershell
Install-Module VMware.PowerCLI
```

**NOTA**: Você será solicitado a instalar o NuGet, siga o padrão ou pressione **Y** e Enter, você será solicitado a confiar em um repositório não confiável, **NÃO** aceite o padrão, mas digite **Y** e pressione Enter.

![APIs4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs4.jpg)

Agora precisamos definir a política de execução como Remote Signed.

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Force
```

![APIs5](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs5.jpg)

Agora você precisará definir a configuração do PowerCLI para ignorar certificados inválidos.

**PASSO IMPORTANTE:**

```powershell
Set-PowerCLIConfiguration -InvalidCertificateAction Ignore -Confirm:$false -WarningAction:SilentlyContinue
```

**NOTA**: Certifique-se de que o "i" em "Ignore" esteja em letras maiúsculas se você não estiver usando Copiar/Colar

![APIs6](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs6.jpg)

Agora precisamos instalar os comandos da CLI do VMware

```powershell
Install-Module -name VMware.VMC -scope AllUsers -Force
```

![APIs7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs7.jpg)

Vamos dar uma olhada rápida nos comandos da CLI do VMware

```powershell
Get-VMCCommand -WarningAction:SilentlyContinue
```

![APIs8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs8.jpg)

Agora precisamos obter seu token de atualização no console VMC. Volte para ou abra o navegador e faça o login **vmc.vmware.com**

Se você ainda não está logado

1. abra uma nova aba
2. Clique no atalho do VMware Cloud on AWS
3. Preencha seu endereço de email
4. Clique em **Next**

    ![APIs9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs9.jpg)

5. Clique no menu suspenso ao lado de seu **Name/Org ID**
6. Clique em **My Account**

    ![APIs10](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs010.jpg)

Agora, criaremos um novo token de atualização para o ID vinculado a essa organização
1. Clique em **API Tokens**.
2. Clique **Generate a New API Token**

    ![APIs011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs011.jpg)

1. Escreva um nome.
2. Selecione o checkbox em **Organization Owner.**
3. Selecione o checkbox em **VMware Cloud on AWS.**
4. Clique **Generate**.

    ![APIs012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs012.jpg)
5.  Clique em **Copy** para guardar.

**Note:*** Certifique-se de salvar este token de atualização em um local seguro para ser usado na próxima seção ao usar APIs no Postman.

Agora, vamos nos conectar ao servidor VMC, inserir o comando abaixo e anexar o token de atualização após o parâmetro -refreshtoken

```powershell
connect-vmc -refreshtoken
```

![APIs13](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs13.jpg)

Agora que estamos conectados à nossa organização VMC por meio do PowerShell, podemos ver a quais Orgs temos acesso usando o seguinte comando

```powershell
Get-VMCorg
```

Observe a Org Display_Name e o ID

![APIs14](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs14.jpg)

Agora que conhecemos a Org Display_Name, podemos encontrar informações sobre os SDDCs dentro da nossa organização.

**NOTA**: substitua # pelo número da sua estação de trabalho

```powershell
Get-VMCSDDC -Org VMC-WS#
```

![APIs15](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs15.png)

Outra coisa legal que você pode fazer é ver as Credenciais Padrão para o seu SDDC

```powershell
Get-VMCSDDCDefaultCredential -org VMC-WS#
```

**NOTA**: substitua # pelo número da sua estação de trabalho

## REST APIs através do Developer Center

Neste módulo, usaremos a VMware Cloud na API REST da AWS para obter algumas informações básicas sobre sua implementação do VMware Cloud na AWS quanto a Organização e SDDC. Para fazer isso, usaremos o novo recurso da Central de Desenvolvedores no VMware Cloud na AWS. Isso foi criado especificamente para se concentrar no uso de APIs e scripts para criar SDDCs, adicionar e remover hosts, além de se conectar e usar o conjunto completo da API do vCenter. Para começar, volte ao seu ambiente VMC.

![DeveloperCenter1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter1.jpg)

Inicie o navegador Google Chrome na sua área de trabalho do Horizon View

![DeveloperCenter2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter2.jpg)

Se você ainda não efetuou login, faça login em sua organização VMware Cloud on AWS.

1. Na guia VMware Cloud on AWS, clique no menu da Central de desenvolvedores

    ![DeveloperCenter3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter3.jpg)

    No Developer Center, existem muitos recursos excelentes para você explorar. Por exemplo, vamos verificar um exemplo de código que foi enviado por um de nossos desenvolvedores de API. Se você rolar por essa tela, verá que há amostras de código para o Postman (uma ferramenta de desenvolvimento da API REST)

    Você também encontrará amostras para Python, PowerCLI e muitos outros. Qualquer um pode contribuir com amostras de código para a comunidade, se isso interessa você vai para <http://code.vmware.com> ou clique em **VMware{code} Sample Exchange**.

2. Clique em *Code Samples* no menu
3. CLique *Download* em "PowerCLI - VMC Example Script"

    ![DeveloperCenter4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter4.jpg)
    Após o download do script

4. Clique na seta suspensa
5. Clique em **Show in Folder**
6. Descompacte o arquivo **PowerCLI-Example-Scripts-master.zip**
7. Abra a pasta **PowerCLI-Example-Scripts-master**
8. Abra a pasta **Scripts**
9. Abra a pasta **VMware_Cloud_on_AWS**
10. Clique com o botão direito do mouse no script **VMC Example Script.ps1**
11. Clique em editar

    Isso abrirá o ambiente do PowerShell ISE. Agora você pode ver os comandos do PowerShell que você usou no módulo anterior, bem como outros comandos que você pode usar com o seu SDDC. Feche as janelas do PowerShell ISE

    ![DeveloperCenter6](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter6.jpg)

    Vamos agora executar alguns comandos simples da API REST embutidos no Developer Center, voltar para o seu navegador
12. Clique no menu do API Explorer
13. Certifique-se de selecionar seu SDDC
14. Clique na seta suspensa ao lado de Organização
15. Clique na seta suspensa ao lado da primeira API "GET"
16. Clique em *Execute*

    ![DeveloperCenter7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter7.jpg)

    O que não fizemos? Nós não colocamos nenhuma autenticação para extrair esses dados. O motivo é que estamos usando a autenticação de sessão para executar esses comandos. Para executar esses comandos em outro aplicativo, como PowerShell ou Postman, você precisará obter seus tokens de recurso e de sessão antes de poder executar esses comandos.

Vamos dar uma olhada na resposta.
17. Aqui você vê o nome alfanumérico da Organização. Que você também pode encontrar em *\#3*
18. A organização *ID*. *NOTA*: Copie o número de ID, sem as aspas, para possível uso na próxima etapa.
19. A organização *Display_Name*
20. A versão da organização

    ![DeveloperCenter8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter8.jpg)

Nesta etapa, obteremos algumas informações sobre nossa organização
21. Clique na seta suspensa por SDDCs
22. Clique em **GET**
23. O ID da organização já deve ser preenchido para você, outro ótimo recurso que os desenvolvedores criaram com base no feedback do cliente. *NOTA*: se esta ID da organização não foi preenchida automaticamente, cole-a.
24. Clique em **Execute**

    ![DeveloperCenter9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter9.jpg)

    Agora vamos olhar para o corpo da resposta25. The creation date of the SDDC
26. O SDDC ID
27. O SDDC state

## Postman

Neste módulo, exploraremos como usar o Postman para executar solicitações da API REST e construir a automação por meio de Collections. Postman é uma ferramenta de API Explorer. Como exemplo, você pode criar variáveis ​​para uso nas APIs, testar a resposta e usar webhooks para integrar com plataformas de colaboração.

![Postman1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman1.jpg)

O Postman é muito fácil de instalar, então vamos começar.

1. Abra uma nova aba e acesse <https://www.getpostman.com>
2. Clique em *Download the App*

    ![Postman2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman2.jpg)
3. Selecione Postman para **Windows (64-bit)**. Clique **Download**. Dê um duplo clique no arquivo baixado, a instalação será executada sem interação.

    *NOTA*: Para finalizar, você pode fechar todas as guias do carteiro no Chrome

    ![Postman3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman3.jpg)

4. Clique no texto: *Skip Signing in and Take me straight to the app*

    ![Postman4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman4.jpg)
5. Não selecione *Show this window on launch*
6. Feche a janela do browser

    ![Postman5](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman5.jpg)

    Volte para a janela do seu navegador, se você não tiver uma guia aberta para o VMware Cloud na AWS, siga as instruções abaixo
7. Navegue em <https://github.com/vmware/vsphere-automation-sdk-rest/archive/master.zip> para fazer o download do SDK REST do vSphere Automation.

    Nossa equipe interna de desenvolvimento de API fez um ótimo trabalho pré-criando SDKs para muitos dos linguagens populares em uso atualmente. Para este módulo, usaremos o SDK para REST para mostrar como você pode importar e reutilizar facilmente algumas Collections pré-criadas para criar suas próprias Collections.

    ![Postman7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman7.jpg)
8.  *This step intentionally left blank*
9.  *This step intentionally left blank*
10. Clique no menu de download
11. Clique em *Open*

    ![Postman8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman8.jpg)
12. Clique em *Extract*
13. Clique em *Extract all*

    ![Postman9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman9.jpg)

    Vamos manter o caminho do arquivo padrão.

14. Desmarque a caixa
15. Clique em *Extract*

Feche a janela do explorador de arquivos.

    ![Postman10](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman10.jpg)

Agora que temos o Postman instalado e nossas amostras de REST em nosso sistema local, vamos importar a coleção VMC e usar algumas solicitações para construir nossa própria coleção.

16. Clique em *Import*
17. Clique em *Choose Files*

    ![Postman11](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman11.jpg)

    Para importar o arquivo json da coleção VMC, fizemos o download anteriormente.
18. Navegue até o diretório que extraímos o arquivo zip para o exercício anterior. Esse diretório deve ser *C:\downloads\vsphere-automation-sdk-rest-master\vsphere-automation-sdk-rest-master\samples\postman*
19. Clique em *VMware Cloud on AWS APIs.postman_collection.json*
20. Clique *Open*

    ![Postman12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman12.jpg)

    Agora precisamos receber nosso token de atualização para nossa Org em VMC. Volte para sua guia VMware Cloud on AWS no seu navegador
21. Clique no menu suspenso ao lado do seu *Name/Org ID*
22. Clique em *My Account*


    Agora, criaremos um novo token de atualização para o código vinculado a essa organização.

    ***NOTA***: Se você já gerou um token, use o mesmo token que foi gerado. Você também pode gerar um novo token, se necessário.

    ![Postman13](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs010.jpg)
23. Clique em **API Tokens**.
24. Clique **Generate a New API Token**

    ![APIs011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs011.jpg)

25. Escreva um nome.
26. Selecione o checkbox em **Organization Owner.**
27. Selecione o checkbox em **VMware Cloud on AWS.**
28. Clique **Generate**.

    ![APIs012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs012.jpg)
29. Clique em **Copy** para guardar.

    ![Postman16](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman16.jpg)

    Volte para o aplicativo Postman. Agora precisamos configurar um ambiente do Postman para uso com o VMC. Um ambiente é onde nós estaremos criando e armazenando nossas variáveis. Essas variáveis ​​podem ser locais ou globais, dependendo do seu uso no Postman. Neste módulo, estaremos usando apenas variáveis ​​locais.

30. Clique em *New*
31. Clique em *Environment*

    ![Postman17](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman17.jpg)
32. Nomeio o ambiente como **VMC**
33. Na coluna Type escreva **refresh_token**
34. Na coluna Value, use CTRL-V para colar seu token de atualização real copiado em uma etapa anterior.
35. Clique em *Add*
36. Feche a janela

    ![Postman18](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman18.jpg)

    Agora defina isso como nosso ambiente padrão.

    *NOTA*: Se você não definir o ambiente padrão como *VMC*, as variáveis ​​criadas não serão acessíveis.
37. Clique na seta suspensa
38. Selecione *VMC*

    ![Postman19](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman19.jpg)

    Agora começaremos a criar nossa própria Collection usando alguma solicitação que veio no SDK que importamos anteriormente.
39. Clique em *Collections*
40. Clique em - *Authentication and Login*
41. Veja como esta solicitação é nossa variável de token de atualização que definimos em uma etapa anterior.

    *NOTA*: Se o ambiente não estiver configurado como VMC, isso ocasionará uma falha porque a variável refresh_token não está definida.
42. Clique em *Send*
43. Agora você verá o token de acesso gerado com o token de atualização. Isto é a resposta ao nosso pedido.

    ![Postman20](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman20.jpg)
41. Clique no icone Eye

    Você verá que nós armazenamos seu token de acesso em uma variável para que possamos usá-lo para chamadas futuras. Como fizemos isso? Nós fizemos um "teste" no corpo da resposta. Você verá como no próximo passo.

    ![Postman21](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman21.jpg)
42. Clique em *Tests*

    A variável access_token foi configurada executando algum código de script java na resposta. Também estamos usando a função setEnvironmentVariable do Postman para criá-la.

    ![Postman22](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman22.jpg)

    Vamos salvar essa solicitação para nossa própria coleção para que possamos usá-la mais tarde.
43. Clique na seta suspensa
44. Clique em *Save As*

    ![Postman23](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman23.jpg)

45. Alterar o Request Name para *Authorize*
46. Alterar o Request description para *Get Access Token*
47. Clique em *+Create Collection*
48. Escreva **Workshop** e clique no *check box*

    ![Postman24](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman24.jpg)
49. Selecione a pasta *Workshop*
50. Clique em *Save to Workshop*

    ![Postman25](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman25.jpg)

    Uma nova janela será aberta indicando que você criou uma nova Collection. Nós não faremos nada aqui neste momento.
51. Feche a janela

    ![Postman26](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman26.jpg)

    Vamos solicitar alguns detalhes da nossa Org para que possamos enviá-los para o Slack.
52. Clique em *Orgs* e *List Orgs*
53. Clique em *Headers*
54. Clique *Send*
55. Você vê aqui como estamos usando a variável **access_token** para o **csp-auth-token**. Isso autorizará nosso pedido. *NOTA*: Este token de acesso só é válido por 30 minutos. Se você executar essa solicitação e receber uma resposta do **400 unauthorized**, volte e execute a solicitação de autorização.
56. Olhe através do corpo da resposta para sua organização **display_name**

    ![Postman27](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman27.jpg)

    Vamos salvar essa solicitação para nossa própria coleção, para que possamos usá-la mais tarde.
57. Clique na seta suspensa
58. Clique em *Save As*

    ![Postman28](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman28.jpg)
59. Modifique o Request name para **Org list**
60. Modifique o Request description para **Get a list of your Orgs**
61. Tenha certeza que *Workshop* está selecionado em **Select a collection or folder to save to:**
62. Clique em *Save to Workshop*

    ![Postman29](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman29.jpg)

    Precisamos substituir o código de teste que acompanha o SDK para que possamos criar uma variável que desejamos usar ao enviar nossa mensagem ao Slack.
63. Clique em Tests
    Copie e cole o código abaixo na seção **Tests**. *NOTA*: Você pode ter que pressionar CTRL-V para colar na caixa de texto.

64. Clique *Send*

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

    Podemos verificar se as variáveis ​​foram criadas e atribuídas valores.
65. Clique no ícone Eye (Olho)
66. Role para baixo para ver se as novas variáveis ​​foram criadas.
    Uma vez verificada, clique no ícone "olho" novamente para fechar a janela

    ![Postman31](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman31.jpg)

    Vamos salvar as alterações que fizemos nessa solicitação.
67. Clique em *Save*

    ![Postman32](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman32.jpg)

    Agora que temos detalhes de nossa Org, vamos enviá-los para ler uma mensagem.

    Para postar no slack, um link precisa ser gerado para o canal slack ao qual queremos postar. Isso já foi feito para você e está listado abaixo. Um dos instrutores terá este canal slack exibido nas telas. Então você pode ver os resultados.

    URL do Slack:
    ```link
    https://hooks.slack.com/services/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
    ```

    Agora precisamos configurar a solicitação:

68. Clique em **+** para uma nova Request
69. Mude request type para *POST*
70. Recorte e cole o URL do canal slack acima para o *address*
71. Selecione *Body*
72. Altere o tipo de formato para *raw*
73. Digite o código abaixo ou recorte e cole na seção Body. *NOTA*: Você pode ter que pressionar CTRL-V para passar para a caixa de texto.

    ```json
    {
      "text" : "{% raw %}Your Org ID is: {{ID}}\nYour Org version is: {{version}}\nAnd your Org state is: {{state}}{% endraw %}",
      "username" : "{% raw %}{{name}}{% endraw %}"
    }
    ```
74. Clique *Send*

    ![Postman33](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman33.jpg)

    Vamos salvar essa solicitação para nossa própria coleção para que possamos usá-la mais tarde.
75. Clique na seta suspensa
76. Clique *Save As*

     ![Postman34](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman34.jpg)
77. Mude o Request name para **Post to Slack**
78. Mude o Request description para **Post some Org details to slack**. Tenha certeza que Workshop está selecionado em *Select a collection or folder to save to:*
79. Clique em *Save to Workshop*

    Verifique e veja se sua solicitação postou o nome, a ID, a versão e o status da sua organização.

    ![Postman35](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman35.jpg)

    A última coisa para mostrar a você com o Postman é a maneira como você pode executar uma Collection para automatizar uma série de tarefas. O que fizemos neste módulo é construir uma Collection. Como você vê na captura de tela, há três tarefas na Collection do Workshop.
80. Clique na seta na janela do Workshop
81. Clique em *Run*

    ![Postman36](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman36.jpg)
82. Clique em *Run Workshop*
83. Tenha certeza que **Environment** está configurado como VMC

    ![Postman37](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman37.jpg)

    Se todo o seu trabalho foi salvo e executado individualmente, eles também devem ser executados aqui.
84. Confira o status de cada solicitação.

Se você tem todos os "200 OK", então você verá outro post no slack para o seu workshop Org.

Por favor, adicione comentários abaixo se você gostaria de dar feedback sobre este laboratório.
