---
layout: single
title: "Trabalhando com o seu SDDC"
date: 2018-09-24
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---
# Introdução

Neste laboratório, começaremos examinando as tarefas básicas que você executará na interface do VMware Cloud on AWS para administrar a plataforma.

## Visualizando o seu SDDC

![SDDC-Network-Login](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc-login.jpg)

Acesse a console do VMware Cloud on AWS no link https://vmc.vmware.com e use as credenciais de login fornecidas **ced##@vmware-hol.com**.

Após o login, você verá dois SDDCs com apenas um nó na interface do usuário, seguindo o nomenclatura Student - ##. Um SDDC é um ambiente dedicado, incluindo vSphere, NSX, vSAN e o vCenter Server. A criação de um SDDC totalmente configurado leva cerca de duas horas, portanto, para as finalidades deste laboratório, já o criamos para você. Este SDDC está na mesma configuração caso tivesse sido implantado por você. Vamos dar uma olhada nas propriedades do SDDC.

![SDDC-Network-01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc01.jpg)

1. Primeiro identique o SDDC designado para você (Student-##).
2. Clique em View Details para abrir as propriedades do SDDC.

![SDDC-Network-02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc02.jpg)

Você irá iniciar com o resumo do SDDC. Existem diversas outras abas disponíveis:
1. **Support**: Você poderá entrar em contato com o Suporte através do seu SDDC ID, Org ID, IP público ou privado do vCenter e a data da criação do seu SDDC.
2. **Settings**: Fornece acesso ao vSphere Client (HTML5), APIs do vCenter, PowerCLI e informa as credenciais para autenticação.
3. **Troubleshooting**: Permite a execução de testes de conectividade para todos os acessos necessários para executar alguns casos de uso selecionados.
4. **Add Ons**: Aqui você irá encontrar os serviços Add-On para o seu ambiente VMware Cloud on AWS como o HCX - Hybrid Cloud Extension e o VMware Site Recovery.
5. **Networking & Security**: Fornece o diagrama completo de configuração dos Gateways de Management e Compute. Será aqui aonde as configurações de redes lógicas, VPNs e regras de Firewall/Segurança serão realizadas. Iremos detalhar a seguir. Clique em Networking & Security para ir a próxima sessão e  aprender mais sobre as configurações de Redes e Segurança no VMware Cloud on AWS.

## Criando uma Rede Lógica

![SDDC-Network-03](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc03.jpg)

Na sessão anterior, você pode verificar as informações de Redes e Segurança sobre seu SDDC.
O VMware Cloud on AWS permite que você possa fácil e rapidamente criar redes lógicas sob demanda. Vamos criar a primeira rede lógica no SDDC.

1. Clique em **Networking & Security** e depois clique em **Segments** para visualizar todos os segmentos de redes lógicas existentes.
2. Clique em **Add Segments** para criar um novo segmento de rede lógica.
3. Para o nome da nova rede, utilize **Demo-Net**
4. No campo Gateway/Prefix Length, utilize 10.10.XX.1/24 (XX representa o seu número de estudante). Este campo representa o Default Gateway do rede e o tamanho do segmento. Para maiores detalhes, veja o endereçamento a seguir.
5. No campo **DHCP**, clique e selecione Enabled para habilitar DHCP para esta rede.
6. Preencha com **10.10.XX.10-10.10.XX.200** para o **DCHP IP Range**. Isto representa a quantidade de endereços IPs que o servidor DHCP irá fornecer para as VMs conectadas nesta rede.
7. Clique em **Save** para criar a rede lógica.

**Nota: Mantenha o padrão do tipo da rede como Routed e não preencha nada no campo DNS suffix.**

<aside class="notice">
<font color="dodgerblue">
<img src="https://s3-us-west-2.amazonaws.com/vmc-workshops-images/info.jpeg" width="25" height="25"> A notação CIDR é uma representação simples de um endereço IP e seu prefixo de roteamento associado. A notação é construída a partir de um endereço IP, um caractere de barra ('/') e um número decimal. O número é a contagem de bits principais na máscara de roteamento, tradicionalmente chamada de máscara de rede. O endereço IP é expresso de acordo com os padrões IPv4 ou IPv6.
<br/>
<br/>
O endereço pode denotar um endereço de interface único e distinto ou o endereço inicial de uma rede inteira. O tamanho máximo da rede é dado pelo número de endereços possíveis com os bits restantes, menos significativos, abaixo do prefixo. A agregação desses bits é geralmente chamada de identificador de host/servidor.
<br/>
<br/>
Por examplo:
<br/>
<br/>
• 192.168.100.14/24 representa o endereço IPV4 192.168.100.14 e seu prefixo de roteamento associado 192.168.100.0, ou equivalentemente, sua máscara de sub-rede 255.255.255.0, que possui 24 bits.
<br/>
<br/>
• O bloco IPV4 192.168.100.0/22 ​​representa os 1024 endereços IPV4 de 192.168.100.0 a 192.168.103.255.
</font>
</aside>

## Verificação da Configuração do Segmento de Rede

![SDDC-Network-04](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc04.jpg)

1. Verifique se o segmento de rede foi adicionado corretamente. Suas informações devem corresponder à área destacada acima.


## Configurar a regra de firewall para o acesso ao vCenter

![SDDC-Network-05](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc05.jpg)

Por padrão, todas as regras de firewall de entrada são definidas como Deny no VMware Cloud na AWS. Para acessar o servidor vCenter, precisaremos configurar uma regra de firewall que permita o acesso de entrada.

**Nota: Na maioria dos ambientes corporativos, você criaria uma VPN ou utilizaria Direct Connect  para permitir regras de firewall de acesso limitado ao vCenter. Neste ambiente, vamos abri-lo para qualquer endereço IP na internet, o que não é recomendado.**

1. Clique em **Gateway Firewall** no lado esquerdo da tela.
2. Se ainda não estiver selecionado, clique em **Management Gateway** para criar regras de firewall que permitam o acesso aos componentes de gerenciamento no SDDC.
3. Clique em **Add New Rule** para adicionar uma nova regra ao gateway.
4. Em **Name** coloque **vCenter Inbound Rule**.
5. Clique **Set Source** para definir a origem na regra de firewall.

### Selecione a origem da regra do firewall

![SDDC-Network-06](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc06.jpg)

1. Clique em **Radio Button** próximo de **Any**.
2. Clique **Save** para salvar a origem definida na regra.

### Configurar a regra de firewall para acesso ao vCenter (Continuação)

![SDDC-Network-07](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc07.jpg)

1. Clique **Set Destination** para iniciar uma nova janela para definir o destino da regra.

### Selecione o destino da regra de firewall

![SDDC-Network-08](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc08.jpg)

1. Clique em **Radio Button** próximo de **System Defined Groups**.
2. Selecione o **Checkbox** próximo de **vCenter**.
3. Clique **Save** para salvar as informações de destino na regra.

### Configurar a regra de firewall para acesso ao vCenter (Continuação)

![SDDC-Network-09](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc09.jpg)

Continue configurando a regra de entrada para o vCenter:

1. Clique na caixa abaixo de **Services** e selecione **HTTPS (TCP 443)** para permitir acesso SSL ao vCenter server.
2. Publique as regras clicando **Publish** para ativar as regras de firewall.

O vCenter agora deve estar acessível de qualquer lugar na internet. Na próxima seção, acessaremos vCenter para configurar máquinas virtuais.

## Logando no vCenter do VMware Cloud on AWS

![SDDC-vcenter-010](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc010.jpg)

As configurações para se conectar ao servidor vCenter associado ao SDDC estão disponíveis na guia de configuração do SDDC. Vamos nos conectar ao servidor vCenter e fazer o login.

1. Click on the **Settings** tab for the SDDC we configured in the last lesson.
2. Click the **arrow** next to Default vCenter User Account to expose the login details. In this lab we will use the default cloudadmin@vmc.local user.
3. Copy the **password** by clicking the two squares next to the password. This will copy it to the consoles clipboard.
4. Click the **arrow** next to **vSphere Client (HTML5)** to expose the URL for vCenter.
5. Click the **URL** link to **open** the vSphere Client in another tab.

1. Clique na aba **Settings** do SDDC que configuramos na última seção.
2. Clique em **arrow** ao lado de Default vCenter User Account para expor os detalhes de login. Neste laboratório, usaremos o usuário cloudadmin@vmc.local padrão.
3. Copie a **password** clicando nos dois quadrados ao lado da senha. Isso irá copiá-lo para a área de transferência.
4. Clique na **URL** do **vSphere Client (HTML5)** para expor a URL do vCenter.
5. Clique na **URL** para **abrir** o vSphere Client em outra guia.

**OBSERVAÇÃO: Se você tiver problemas de login abaixo, poderá clicar nas duas caixas ao lado do URL abaixo para colar o URL em uma janela anônima. Isso não deve ser necessário normalmente.**

### Logando no vSphere Client (HTML5)

Para realizar o login no vSphere Client (HTML5):

![SDDC-vcenter-011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc011.jpg)

1. Em User name escrever **cloudadmin@vmc.local.**
2. Clique com botão direito em **Password** para colar a senha copiada no passo anterior.
3. Clique em **Login**.

### vSphere Client (HTML5)

![SDDC012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc012.jpg)

Você está logado no VMware Cloud AWS vCenter Server com o usuário cloudadmin@vmc.local.

## Criando uma Content Library

Content libraries são objetos para templates de VM, vApps e outros tipos de arquivos, como imagens ISO.

Você pode criar uma Content Library no vSphere Client (HTML5) e preenchê-la com templates, que você pode usar para implantar máquinas virtuais ou vApps no ambiente VMware Cloud on AWS ou se já tiver uma Content Library em seu Data Center, você pode usar a Content Library para importar conteúdo para o seu SDDC.

Você pode criar dois tipos de Content Library: Local ou Subscribed.

**Local**

Você pode usar uma Local Content Library para armazenar itens em uma única instância do vCenter Server. Você pode publicar a Local Content Library para que os usuários de outros vCenter Servers possam se inscrever nela. Quando você publica uma Content Library externamente, você pode configurar uma senha para autenticação.

Templates de VM e de vApps são armazenados como formatos de arquivo OVF na Content Library. Você também pode carregar outros tipos de arquivo, como imagens ISO, arquivos de texto e assim por diante, em uma Content Library.

**Subscribed**

Você se inscreve em uma Content Library publicada criando uma Subscribed Content Library. Você pode criar a Subscribed Content Library na mesma instância do vCenter Server em que a Content Library publicada está ou em um sistema diferente do vCenter Server. No assistente Create Content Library, você tem a opção de baixar todo o conteúdo da Content Library publicada imediatamente após a criação da Subscribed Content Library ou de baixar apenas os metadados dos itens da Content Library publicada e, posteriormente, fazer o download do conteúdo completo apenas dos itens pretendo usar.

Para garantir que o conteúdo de uma Subscribed Content Library esteja atualizado, a Content Library é sincronizada automaticamente com a Content Library de origem em intervalos regulares. Você também pode sincronizar manualmente as Subscribed Content Library.

Você pode usar a opção para baixar conteúdo da Content Library imediatamente ou somente quando necessário para gerenciar seu espaço de armazenamento.

A sincronização de uma Subscribed Content Library configurada com a opção de baixar todo o conteúdo da Content Library publicada imediatamente sincroniza os metadados do item e o conteúdo do item. Durante a sincronização, os itens da Content Library que são novos para a Subscribed Content Library são totalmente transferidos por download para o local de armazenamento da Content Library.

A sincronização de uma Subscribed Content Library configurada com a opção de baixar o conteúdo apenas quando necessário sincroniza apenas os metadados dos itens da Content Library da Subscribed Content Library publicada e não faz o download do conteúdo dos itens. Isso economiza espaço de armazenamento. Se você precisar usar um item da Content Library, precisará sincronizar esse item. Depois de terminar de usar o item, você pode excluir o conteúdo do item para liberar espaço no armazenamento. Para Subscribed Content Library configuradas com a opção de baixar o conteúdo apenas quando necessário, a sincronização da Subscribed Content Library baixa apenas os metadados de todos os itens da Content Library de origem, enquanto a sincronização de um item da Content Library faz o download do conteúdo completo desse item para o armazenamento.

Se você usar uma Subscribed Content Library, só poderá utilizar o conteúdo, mas não poderá contribuir com o conteúdo. Apenas o administrador da Subscribed Content Library pode gerenciar os modelos e arquivos.

### Acessando Content Libraries no vSphere Client

![SDDC013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc013.jpg)

1. Clique em **Menu**
2. CLique em **Content Libraries**

### Inscrevendo-se em uma Subscribed Content Library

![SDDC014](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc014.jpg)

1. Na janela do Content Library, clique **+ (plus)**  para adicionar uma nova Content Library.

![SDDC015](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc015.jpg)

1. Escreva **VMC Content Library** no campo Name.
2. Clique **Next**.

![SDDC016](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc016.jpg)

1. Selecione o botão de opção ao lado de **Subscribed content library**.
2. Em **Subscription URL** digite o seguinte: https://vmc-elw-vms.s3-accelerate.amazonaws.com/lib.json
3. Deixe a caixa de seleção **desmarcada** ao lado de **Enabled Authentication**.
4. Certifique-se de que o conteúdo do Download esteja definido como **immediately**.
5. Clique em **Next** para continuar.

![SDDC017](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc017.jpg)

1. Clique em **WorkloadDatastore** para o Datastore da Content Library.
2. Clique em **Next**.

![SDDC018](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc018.jpg)

1. Clique em **Finish**.

**Nota: Dependendo do tamanho e do número de templates, pode demorar um pouco para sincronizar o conteúdo. Esta Content Library deve levar apenas alguns minutos para sincronizar.**

## Criando uma Máquina Virtual

![SDDC-deploy-vm-013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc013.jpg)

Na janela do vSphere Client já aberta, abra a Content Library:

1. Clique **Menu**.
2. Clique em **Content Libraries**.

### Selecione a Content Library

![SDDC-deploy-vm-028](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc028.jpg)

1.  Clique em **VMC Content Library** que foi sincronizada anteriormente.

### Criando uma Máquina Virtual a partir de um Template

![SDDC-deploy-vm-029](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc029.jpg)

1. Clique em **Templates** para acessar o template sincronizado da Content Library.
2. Clique com botão direito em **photoapp-u** template para exportar.
3. Clique em **New VM from This Template** para criar uma máquina virtual a partir deste template.

### Selecione o nome da Máquina Virtual e Localização

![SDDC-deploy-vm-030](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc030.jpg)

1. Escreva **webserver01** para o nome da Máquina Virtual.
2. Clique na **flecha** próximo ao SDDC-Datacenter para expor as pastas disponíveis.
3. No VMware Cloud on AWS Customer, as Máquinas Virtuais devem ser colocadas na pasta Workloads (ou sub-pastas da pasta Workloads).  Clique na pasta **Workloads**.
4. Selecione o **Checkbox** próximo de **Customize the operating system**.
5. Clique **Next** para continuar.

###  Customização da Máquina Virtual

![SDDC-deploy-vm-031](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc031.jpg)

1. Clique **Next** para continuar.

**Não utilize nenhuma customização**

### Selecione o Resource Pool

![SDDC-deploy-vm-032](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc032.jpg)

1. Clique na **flecha** próximo de Cluster-1 para expor os Resource Pools disponíveis.
2. No VMware Cloud on AWS, máquinas virtuais devem ser provisionadas no **Compute-ResourcePool** (or Sub ResourcePool).  Clique **Compute-ResourcePool**.
3. Clique **Next** para continuar.

### Revendo os detalhes do Template

![SDDC-deploy-vm-033](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc033.jpg)

Revise os detalhes do template a ser implantado. Pode haver um aviso de segurança exibido, mas você pode ignorar para a finalidade deste laboratório.

1. Clique **Next** para continuar.

### Selecione o Storage

![SDDC-deploy-vm-034](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc034.jpg)

Cada VMware Cloud no AWS SDDC incluirá dois datastores para separar as cargas de trabalho de gerenciamento e de clientes. Todas as cargas de trabalho do cliente devem ser colocadas no Datastore chamado WorkloadDatastore.

1. Clique **workloadDatastore** para selecionar o datastore aonde as máquinas virtuais serão provisionadas.
2. Clique **Next** para continuar.

### Selecionando a Rede Lógica para a Máquina Virtual

![SDDC-deploy-vm-035](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc035.jpg)

Usaremos a rede lógica criada em um exercício anterior para essas Máquinas Virtuais.

1. Clique na **flecha** em Destination Network para selecionar a rede da Máquina Virtual.
2. Clique **Demo-Net** para selecionar a rede criada anteriormente.
3. Clique **Next** para continuar.

### Complete a criação da Máquina Virtual

![SDDC-deploy-vm-036](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc036.jpg)

1. Revise as informações e clique em **Finish** para criar a Máquina Virtual

Deve levar alguns minutos para a Máquina Virtual ser criada. Continue no próximo exercício para clonar essa Máquina Virtual e criar um segundo servidor Web.

##  Clonando uma Máquina Virtual

Neste exercício, você clonará a Máquina Virtual criada no exercício anterior para criar um segundo servidor Web.

### Verificando as Máquinas Virtuais e Templates

![SDDC-clone-vm-037](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc037.jpg)

1. Valide a implantação da Máquina Virtual concluída no exercício anterior procurando a tarefa **Deploy OVF Template** e verificando se está **Complete**.
2. Se completada, clique em **Menu**.
3. Clique **VMs and Templates** para navegar pelas Máquinas Virtuais e Templates.

### Selecione e Inicie a Máquina Virtual Webserver01

![SDDC-clone-vm-038](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc038.jpg)

Antes de podermos clonar o servidor web, primeiro precisamos ligar a Máquina Virtual:

1. Clique na **flecha** perto de **SDDC-Datacenter** para expandir as sub-pastas.
2. Clique na **flecha** perto de workloads para expor **webserver01**
3. Clique na Máquina Virtual **webserver01**
4. Clique na **flecha verde** para iniciar a Máquina Virtual.

**Observação: aguarde até que a Máquina Virtual esteja totalmente ligada antes de prosseguir para a próxima etapa.**

<aside class="notice">
<font color="dodgerblue">
<img src="https://s3-us-west-2.amazonaws.com/vmc-workshops-images/info.jpeg" width="25" height="25"> Se o servidor da Web não se conectar à rede e não receber o endereço IP do DHCP, verifique se a NIC está conectada clicando com o botão direito do mouse em <b> webserver01 </b> e, em seguida, em <b> Edit Settings </b> e verifique se a caixa de seleção ao lado de Connected está selecionada. Pode ser necessário repetir esta etapa para o servidor webserver02.
</font>
</aside>

### Clonando a Máquina Virtual

![SDDC-clone-vm-039](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc039.jpg)

Vamos agora iniciar o processo de clonagem desta máquina virtual.

1. Clique com o botão direito em **webserver01** para abri o menu de opções.
2. Clique em **Clone** para expor um segundo menu com opções.
3. Clique **Clone to Virtual Machine** para iniciar o Wizard de Clonagem.

### Selecione o nome da Máquina Virtual e Pasta

![SDDC-clone-vm-040](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc040.jpg)

1. Escreva **webserver02** para o nome da Máquina Virtual.
2. No VMware Cloud on AWS Customer, as Máquinas Virtuais devem ser colocadas na pasta Workloads (ou sub-pastas da pasta Workloads).  Clique na pasta **Workloads**.
3. Clique **Next** para continuar.

### Selecione o Resource Pool

![SDDC-clone-vm-041](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc041.jpg)

1. No VMware Cloud on AWS, máquinas virtuais devem ser provisionadas no **Compute-ResourcePool** (or Sub ResourcePool).  Clique **Compute-ResourcePool**.
2. Clique **Next** para continuar.

### Selecione o Storage

![SDDC-clone-vm-042](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc042.jpg)

1. Clique **workloadDatastore** para selecionar o datastore aonde as máquinas virtuais serão provisionadas.
2. Clique **Next** para continuar.

### Selecionando as opções de Clonagem

![SDDC-clone-vm-043](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc043.jpg)

Vamos agora definir as opções para clonagem. Precisaremos personalizar o sistema operacional para alterar o nome do servidor e também ligar a máquina virtual após a conclusão da clonagem.

1. Clique no checkbox perto de **Customize the operating system**.
2. Clique no checkbox perto de **Power on virtual machine after creation**.
3. Clique **Next** para continuar.

### Customização da Máquina Virtual

![SDDC-clone-vm-044](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc044.jpg)

1. Clique **Next** para continuar.

**Não utilize nenhuma customização**

### Complete a Criação da Máquina Virtual

![SDDC-clone-vm-045](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc045.jpg)

1. Revise as informações para precisão e clique em **Finish** para clonar a máquina virtual.

**NOTA: Deve demorar alguns minutos para a máquina virtual clonar. Continue para o próximo exercício para aprender a proteger as cargas de trabalho no VMware Cloud na AWS.**

<aside class="notice">
<font color="dodgerblue">
<img src="https://s3-us-west-2.amazonaws.com/vmc-workshops-images/info.jpeg" width="25" height="25"> Se o servidor da Web não se conectar à rede e não receber o endereço IP do DHCP, verifique se a NIC está conectada clicando com o botão direito do mouse em <b> webserver01 </b> e, em seguida, em <b> Edit Settings </b> e verifique se a caixa de seleção ao lado de Connected está selecionada.
</font>
</aside>

## Testando a conectividade entre as Máquinas Virtuais

Neste exercício, testaremos a conectividade entre o webserver01 e o webserver02, que criamos nos exercícios anteriores.

### Abrindo a Console do Webserver01

![SDDC-test-vm-046](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc046.jpg)

Precisamos abrir uma sessão de console para o webserver01 para validá-la e comunicar-se com o webserver02.

1. No vSphere Client (HTML5), clique em Webserver01 para focalizar.
2. Clique na caixa preta abaixo de Summary no meio da tela. Isso tentará iniciar uma sessão do console, mas poderá falhar porque o pop-up foi bloqueado. Se isso ocorrer, siga as etapas de 3 a 6, caso contrário, vá para a próxima sessão.
3. Clique no ícone com o pequeno x vermelho na barra de endereço do Chrome para abrir a caixa de diálogo do bloqueador de pop-up.
4. Clique no botão de opções ao lado de Always allo pop-ups em https://vcenter.sddc-xx-xx-xxxx.vmwarevmc.com
5. Clique no botão Done.
6. Volte para a caixa preta abaixo do Summary e clique novamente. A sessão do console deve ser iniciada em uma nova guia.

### Descobrindo o endereço IP do Webserver02

![SDDC-test-vm-047](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc047.jpg)

Antes que possamos testar a conectividade entre os dois servidores, precisamos encontrar o endereço IP do webserver02.

1. Clique no **Aba do Chrome** do vSphere Client (HTML5) para voltar ao foco.
2. Clique na máquina virtual **webserver02**.
3. Anote o **IP Address** para o webserver02 no meio da tela. Isso será necessário na próxima etapa.
4. Clique na **Aba do Chrome** da sessão de console do webserver01 para voltar ao foco.

### Logando e Pingando Webserver02

![SDDC-test-vm-048](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc048.jpg)

Agora que temos o endereço IP para o Webserver02, vamos configurar um ping contínuo para o servidor para verificar a comunicação.

Antes de começar clique em qualquer lugar dentro da janela do console para colocá-lo em foco

1. No prompt de login, digite **root** e pressione Enter.
2. No prompt de senha, digite **VMware1!** e pressione Enter.
3. No prompt do console, insira **ping 10.10.xx.xxx** e pressione Enter. O terceiro octeto é baseado no número do aluno e o último octeto do endereço IP, na maioria dos casos, será 101, mas verifique isso na sua configuração.
4. Verifique se os pings foram bem-sucedidos.

**NOTA: Por favor, deixe este ping e console de janela aberta para a próxima lição. Vamos revisá-lo para verificar se os servidores da Web não podem mais se comunicar.**

Parabéns! Agora você implantou dois servidores da Web no VMware Cloud no AWS SDDC e verificou que eles podem se comunicar uns com os outros. Na próxima lição, criaremos regras de firewall para impedir que os servidores se comuniquem entre si e também tornem o webserver02 acessível a partir da Internet.

## Configurando so Serviços Avançados de Redes no VMware Cloud on AWS

Os Serviços Avançados de Redes no VMware Cloud no AWS agora estão disponíveis para novas implantações do SDDC.

### Serviços Avançados de Redes e Segurança

![DFW-01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw01.jpg)

Com os Serviços de Redes Avançados no VMware Cloud no AWS, os usuários têm a capacidade de implementar a microssegmentação com o Firewall Distribuído. Políticas de segurança granulares podem ser aplicados no nível da Máquina Virtual permitindo segmentação na mesma rede L2 ou em redes L3 separadas. Isso é mostrado no diagrama acima.

Toda a configuração de rede e segurança agora é feita por meio do console VMware Cloud on AWS por meio da guia Networking and Security, incluindo a criação de segmentos de rede. Isso facilita a operação e o gerenciamento, tendo acesso à rede e à segurança por meio da console.

### Firewall Distribuído

![DFW-02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw02.jpg)

Na captura de tela acima, é possível ver, além da capacidade de criar várias seções, que os usuários podem organizar regras de firewall distribuído em grupos (regras de emergência, regras de infraestrutura, regras de ambiente e regras de aplicativo. As regras são atingidas de cima para baixo).

### Grupos de Segurança

![DFW-03](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw03.jpg)

Além dos novos recursos do Firewall Distribuído, o agrupamento de objetos agora pode ser aproveitado nas políticas de segurança. Os grupos de segurança suportam os seguintes critérios / construções de agrupamento:

* Endereço de IP
* Instância de Máquina Virtual
* Critérios de correspondência do nome da Máquina Virtual
* Critérios de correspondência da tag de segurança

Como mostrado acima, os grupos de segurança podem ser criados em grupos de Workloads ou grupos de gerenciamento. Grupos de Workloads podem ser usados ​​em políticas de firewall distribuída ou borda, e os grupos de gerenciamento podem ser usados ​​em políticas de firewall de borda. Grupos de gerenciamento suportam apenas endereços IP, já que esses grupos são baseados em infraestrutura. Grupos de gerenciamento predefinidos já existem para o vCenter, os hosts ESXi e o NSX Manager. Os usuários também podem criar grupos aqui com base no endereço IP para hosts ESXi no Data Center, vCenter e outros dispositivos de gerenciamento.

### Verificando as Máquinas Virtuais em um Grupo de Segurança

![DFW-04](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw04.jpg)

Aqui você pode ver que implantamos algumas Máquinas Virtuais no vCenter e você pode ver as Máquinas Virtuais no inventário dentro do console. Além disso, identificamos as Máquinas Virtuais com as tags de segurança Web, App e DB, respectivamente.

### Colocando tags nas Máquinas Virtuais

![DFW-tag-05](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw05.jpg)

Por meio do console VMware Cloud on AWS, podemos aplicar tags de segurança a máquinas virtuais e agrupá-las.

Agora, voltaremos para o console VMware Cloud on AWS.

1. Clique na aba **VMware Cloud on AWS** e faça o login com as informações que você forneceu se sua sessão expirou.
2. Clique em **View Details** para acessar os detalhes do SDDC.

### Edite as tags para Webserver01

Agora, começaremos a marcar as máquinas virtuais com tags de segurança.

![DFW-tag-06](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw06.jpg)

1. Clique na guia **Networking & Security** para acessar a configuração de rede.
2. No lado esquerdo da tela, clique em **Groups**.
3. Em Groups, clique em **Virtual Machines** para acessar as máquinas virtuais que fazem parte do SDDC.
4. Localize **webserver01** e clique nos três pontos verticais e clique em **Edit**.

### Adicionando tag de segurança para webserver1

![DFW-tag-07](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw07.jpg)

1. Em Tags, insira **Web** para o webserver01.
2. Clique em **Save** para confirmar as alterações.

### Editando as tags para Webserver02

![DFW-tag-08](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw08.jpg)

Vamos agora marcar o Webserver02 com a mesma tag da Web. Usaremos isso para criar um grupo para os dois servidores da web.

1. Localize **webserver02** e clique nos três pontos verticais e clique em **Edit**.

### Adicionando tag de segurança para webserver2

![DFW-tag-09](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw09.jpg)

1. Em Tags, insira **Web** para o webserver02.
2. Clique em **Save** para confirmar as alterações.

### Criando um Grupo Dinâmico

![DFW-tag-010](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw010.jpg)

Os grupos podem ser usados ​​no VMware Cloud no AWS  para agrupar máquinas virtuais e simplificar a configuração de refras de firewall. Neste exercício, agruparemos os dois servidores da Web em um grupo e criaremos uma regra de firewall para bloquear a comunicação entre eles. Em uma aplicação tradicional adequadamente arquitetada, geralmente não há necessidade de servidores na camada da Web se comunicarem.

Agora, criaremos um grupo de servidores da Web com base na tag de segurança dinâmica que aplicamos anteriormente.

1. Clique em **Workload Groups**.
2. Clique em **Add Group**.
3. Em Nome, insira **Web** para o nome do grupo.
4. Em Member Type, clique no **menu suspenso** e selecione **Membership Criteria**.
5. Em Members, clique em **Set Membership Criteria**.

### Adicinando um critério de agrupamento

![DFW-tag-011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw011.jpg)

Vamos agora adicionar os critérios para agrupar máquinas com base na tag de segurança.

1. Clique em **+ Add Criteria**.
2. Em Property, clique no **drop-down** e selecione **Tag**.
3. Em Value, escreva **Web**.
4. Clique **Save** para continuar.

### Salve as Mudanças

![DFW-tag-012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw012.jpg)

1. Clique **Save** para aplicar as mudanças.

### Verifique os Membros

![DFW-tag-013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw013.jpg)

Agora podemos validar a associação do grupo está funcionando conforme o esperado.

1. Clique nos **três pontos verticais** ao lado do grupo  Web.
2. Clique em **View Members** para mostrar os membros atuais do grupo dinâmico.

### Validando os Membros do Grupo

![DFW-tag-014](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw014.jpg)

1. Valide que **webserver01** e **webserver02** aparecem como membros do grupos. Se não, volte e verifique se não há erros de digitação.
2. Clique **Close**.

Agora que esse grupo foi criado, você pode adicionar novos membros facilmente simplesmente aplicando uma tag de segurança.

### Criando uma Seção de Regras de Firewall

![DFW-tag-015](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw015.jpg)

Agora que criamos nosso grupo dinâmico, vamos criar uma regra de firewall para bloquear o acesso entre os servidores web.

1. Clique **Distributed Firewall**.
2. Clique **Application Rules**.
3. Clique **Add New Section** para criar uma nova seção para a regra. Essa funcionalidade permite agrupar as regras logicamente para simplificar a operação do ambiente.
4. Em Name, coloque **Web Tier**.
5. Clique **Publish** para salvar as mudanças.

### Adicione uma Regra de Firewall

![DFW-tag-016](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw016.jpg)

Agora que temos a seção criada, agora podemos adicionar uma regra de firewall.

1. Clique na **seta** ao lado da seção Web.
2. Clique em **Add New Rule** no menu acima das regras.
3. Em Nome, insira **Block Web to Web**.
4. Em Action, clique no menu suspenso e selecione **Drop**.
5. Em Sources, clique em **Any**.

### Selecione a Origem para a Regra de Firewall

![DFW-tag-017](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw017.jpg)

1. Clique no **checkbox** perto de Web.
2. Clique **Save** para salvar as regras.

### Adicione o Destino para a Regra de Firewall

![DFW-tag-018](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw018.jpg)

1. Em Destinations clique **Any**.

### Selecione o Destino

![DFW-tag-019](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw019.jpg)

1. Clique no **checkbox** perto de Web.
2. Clique **Save** para salvar as regras.

### Publique a Regra de Firewall

![DFW-tag-020](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw020.jpg)

1. Clique **Publish** para confirmar a regra e começar a bloquear o tráfego entre os servidores da web.

### Testando a regra do firewall distribuído

![DFW-tag-021](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw021.jpg)

Você ainda deve ter a sessão de console aberta do exercício anterior para webserver01 e deve estar executando um comando ping.
1. Clique na aba do **webserver01**.
2. Ping webserver02 com IP 10.10.xx.xxx.

Os pings devem ter parado de responder, o que significa que as regras de firewall distribuídas foram aplicadas corretamente. Esta simples demonstração deve dar uma idéia do poder do firewall distribuído.

## Conclusão

Neste módulo, exploramos a configuração da configuração de um VMware Cloud no AWS SDDC, incluindo a utilização da Content Library, a implantação de máquinas virtuais e de regras de firewall.

## Single Host SDDC

Se você gostou do Lab e quiser continuar a experimentar e testar os recursos do VMware Cloud on AWS, utilize o QR Code abaixo para iniciar sua experiência de 1 host.

![DFW-tag-022](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/dfw022.jpg)

Você concluiu este módulo.

Por favor, adicione comentários abaixo se você gostaria de dar feedback sobre este exercício.
