---
title: Distribuir um aplicativo do Windows 10 por um servidor de IIS
description: Este tutorial demonstra como configurar um servidor IIS, verifique se seu aplicativo web pode hospedar pacotes de aplicativos e invocar e usar o instalador de aplicativos com eficiência.
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, uwp, o instalador do aplicativo, AppInstaller, carregar, relacionadas a pacotes definidos, opcionais, servidor do IIS
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: bacf7ab3125d651ef30320072dd45a94bffc677f
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828742"
---
# <a name="distribute-a-windows-10-app-from-an-iis-server"></a>Distribuir um aplicativo do Windows 10 por um servidor de IIS

Este tutorial demonstra como configurar um servidor IIS, verifique se seu aplicativo web pode hospedar pacotes de aplicativos e invocar e usar o instalador de aplicativos com eficiência.

O aplicativo do Instalador de aplicativo permite que desenvolvedores e profissionais do setor de TI distribuam aplicativos do Windows 10 hospedando-os em sua própria Rede de disponibilização de conteúdo (CDN. Isso é útil para empresas que não desejam ou precisam publicar seus aplicativos na Microsoft Store, mas ainda querem aproveitar a plataforma de empacotamento e implantação do Windows 10. 

## <a name="setup"></a>Configuração

Para ir com êxito por meio com este tutorial, você precisará do seguinte:

1. Visual Studio 2017  
2. Ferramentas de desenvolvimento Web e o IIS 
3. Pacote de aplicativo do Windows 10 - o pacote do aplicativo que você distribuirá

Opcional: [Projeto Starter](https://github.com/AppInstaller/MySampleWebApp) no GitHub. Isso é útil se você não tiver pacotes de aplicativos para trabalhar com, mas ainda desejar aprender a usar esse recurso.

## <a name="step-1---install-iis-and-aspnet"></a>Etapa 1: instalar o IIS e ASP.NET 

[Serviços de informações da Internet](https://www.iis.net/) é um recurso do Windows que pode ser instalado por meio do menu Iniciar. Na **menu Iniciar** pesquise **ativar ou desativar recursos do Windows ativar**.

Localize e selecione **serviços de informações da Internet** para instalar o IIS.

> [!NOTE]
> Você não precisa selecionar todas as caixas de seleção em serviços de informações da Internet. Apenas aqueles selecionados quando você faz check **serviços de informações da Internet** são suficientes.

Você também precisará instalar o ASP.NET 4.5 ou superior. Para instalá-lo, localize **serviços de informações da Internet -> World Wide Web Serviços -> recursos de desenvolvimento de aplicativo**. Selecione uma versão do ASP.NET que é maior que ou igual ao ASP.NET 4.5.

![Captura de tela de instalação de recurso do ASP.NET](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>Etapa 2: instalar o Visual Studio 2017 e ferramentas de desenvolvimento da Web 

[Instalar o Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) se você ainda não instalou-lo. Se você já tiver o Visual Studio 2017, certifique-se de que as cargas de trabalho a seguir estão instaladas. Se as cargas de trabalho não estiverem presentes em sua instalação, acompanhe usando o instalador do Visual Studio (encontrado no menu Iniciar).  

Durante a instalação, selecione **ASP.NET e desenvolvimento Web** e quaisquer outras cargas de trabalho que você está interessado. 

Depois que a instalação for concluída, inicie o Visual Studio e crie um novo projeto (**arquivo** -> **novo projeto**).

## <a name="step-3---build-a-web-app"></a>Etapa 3: criar um aplicativo Web

Inicie o Visual Studio 2017 como **administrador** e crie um novo **Visual C# aplicativo Web** do projeto com um **vazia** modelo de projeto. 

![Captura de tela de criação de um novo projeto web](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>Etapa 4: configurar o IIS com nosso aplicativo Web 

No Gerenciador de soluções, clique com botão direito no projeto raiz e selecione **propriedades**.

Nas propriedades de aplicativo da web, selecione o **Web** guia. No **servidores** , escolha **Local IIS** na lista suspensa menu e clique em **Create Virtual Directory**. 

![Captura de tela da guia web nas propriedades do projeto](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>Etapa 5: adicionar um pacote do aplicativo a um aplicativo web 

Adicione o pacote do aplicativo que você pretende distribuir para o aplicativo web. Você pode usar o pacote do aplicativo que faz parte do fornecido [pacotes de projeto starter](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages) no GitHub, se você não tiver um pacote do aplicativo disponível. O certificado (MySampleApp.cer) que o pacote usou também faz parte da amostra no GitHub. Você deve ter o certificado instalado em seu dispositivo antes de instalar o aplicativo (etapa 9).

No aplicativo da web de projeto starter, uma nova pasta foi adicionada ao aplicativo web chamado **pacotes** que contém os pacotes de aplicativo a ser distribuído. Para criar a pasta no Visual Studio, o botão direito do mouse no nó do projeto no Solution Explorer, selecione **Add** -> **nova pasta** e nomeie-a **pacotes**. Para adicionar pacotes de aplicativos para a pasta, clique com botão direito do **pacotes** pasta e selecione **Add** -> **Item existente...**  e navegue até o local do pacote de aplicativo. 

![Captura de tela da adição de um pacote](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>Etapa 6: criar uma página da Web

Este aplicativo web de exemplo usa HTML simples. Você é livre para compilar seu aplicativo web conforme exigido por suas necessidades. 

Clique com botão direito no projeto raiz do Gerenciador de soluções, selecione **Add** -> **Novo Item**e adicione um novo **página HTML** do **Web**seção.

Depois de criar a página HTML, clique com o botão direito na página HTML no Gerenciador de soluções e selecione **definir como página inicial**.  

Clique duas vezes no arquivo HTML para abri-lo na janela do editor de código. Neste tutorial, apenas os elementos necessários na página da web para invocar o aplicativo do instalador de aplicativo com êxito para instalar um aplicativo do Windows 10 serão usados. 

Inclua o seguinte código HTML na página da web. A chave para invocar o instalador de aplicativo com êxito é usar o esquema personalizado que o instalador de aplicativo registra com o sistema operacional: `ms-appinstaller:?source=`. Consulte o exemplo de código abaixo para obter mais detalhes.

> [!NOTE]
> Verifique se o caminho de URL especificado depois que o esquema personalizado corresponde à Url do projeto na guia web de sua solução de VS.
 
```HTML
<html>
<head>
    <meta charset="utf-8" />
    <title> Install Page </title>
</head>
<body>
    <a href="ms-appinstaller:?source=http://localhost/SampleWebApp/packages/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>Etapa 7: configurar o aplicativo web para tipos MIME do pacote de aplicativo

Abra o **Web. config** arquivo do Gerenciador de soluções e adicione as seguintes linhas dentro a `<configuration>` elemento. 

```xml
<system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extension-->
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/appx" />
      <mimeMap fileExtension=".msix" mimeType="application/msix" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/appxbundle" />
      <mimeMap fileExtension=".msixbundle" mimeType="application/msixbundle" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/appinstaller" />
    </staticContent>
</system.webServer>
```

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>Etapa 8 - adicionar isenção de loopback para o instalador do aplicativo

Devido ao isolamento de rede, aplicativos do Windows 10 como o instalador de aplicativo são restritos a usar endereços de loopback IP, como http://localhost/. Ao usar o servidor IIS local, o instalador do aplicativo deve ser adicionado à lista de isenção de loopback. 

Para fazer isso, abra **Prompt de comando** como um **administrador** e insira o seguinte: ' ' linha de comando CheckNetIsolation.exe LoopbackExempt - a-n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

To verify that the app is added to the exempt list, use the following command to display the apps in the loopback exempt list: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

Você deve encontrar `microsoft.desktopappinstaller_8wekyb3d8bbwe` na lista.

Quando a validação de local de instalação do aplicativo por meio do instalador do aplicativo for concluída, você pode remover a isenção de loopback que você adicionou nesta etapa por:

```Command Line CheckNetIsolation.exe LoopbackExempt -d -n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

## Step 9 - Run the Web App 

Build and run the web application by clicking on the run button on the VS Ribbon as shown in the image below:

![Screenshot of running web app in Visual Studio](images/run.png)

A web page will open in your browser:

![Screenshot of installing app from web page](images/web-page.png)

Click on the link in the web page to launch the App Installer app and install your Windows 10 app package.


## Troubleshooting issues

### Not sufficient privilege 

If running the web app in Visual Studio displays an error such as "You do not have sufficient privilege to access IIS web sites on your machine", you will need to run Visual Studio as an administrator. Close the current instance of Visual Studio and reopen it as an admin.

### Set start page 

If running the web app causes the browser to load with an HTTP 403.14 - Forbidden error, it's because the web app doesn't have a defined start page. Refer to Step 6 in this tutorial to learn how to define a start page.
