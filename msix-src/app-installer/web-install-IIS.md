---
title: Distribuir um aplicativo do Windows 10 por um servidor de IIS
description: Este tutorial demonstra como configurar um servidor IIS, verificar se seu aplicativo Web pode hospedar pacotes de aplicativos e invocar e usar o instalador de aplicativos com eficiência.
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, UWP, instalador de aplicativos, AppInstaller, Sideload, conjunto relacionado, pacotes opcionais, servidor IIS
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: bacf7ab3125d651ef30320072dd45a94bffc677f
ms.sourcegitcommit: 536d6969cde057877ecdd8345cfb0dc12c9582f2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78909617"
---
# <a name="distribute-a-windows-10-app-from-an-iis-server"></a>Distribuir um aplicativo do Windows 10 por um servidor de IIS

Este tutorial demonstra como configurar um servidor IIS, verificar se seu aplicativo Web pode hospedar pacotes de aplicativos e invocar e usar o instalador de aplicativos com eficiência.

O aplicativo do Instalador de aplicativo permite que desenvolvedores e profissionais do setor de TI distribuam aplicativos do Windows 10 hospedando-os em sua própria Rede de disponibilização de conteúdo (CDN. Isso é útil para empresas que não desejam ou precisam publicar seus aplicativos na Microsoft Store, mas ainda querem aproveitar a plataforma de empacotamento e implantação do Windows 10. 

## <a name="setup"></a>Instalação

Para percorrer com êxito este tutorial, você precisará do seguinte:

1. Visual Studio 2017  
2. Ferramentas de desenvolvimento da Web e IIS 
3. Pacote de aplicativos do Windows 10-o pacote do aplicativo que será distribuído

Opcional: [Projeto inicial](https://github.com/AppInstaller/MySampleWebApp) no GitHub. Isso é útil se você não tem pacotes de aplicativos para trabalhar, mas ainda gostaria de aprender a usar esse recurso.

## <a name="step-1---install-iis-and-aspnet"></a>Etapa 1-instalar o IIS e o ASP.NET 

[Serviços de informações da Internet](https://www.iis.net/) é um recurso do Windows que pode ser instalado por meio do menu iniciar. No **menu iniciar** , procure **Ativar ou desativar recursos do Windows**.

Localize e selecione **serviços de informações da Internet** para instalar o IIS.

> [!NOTE]
> Você não precisa marcar todas as caixas de seleção em Serviços de Informações da Internet. Somente aqueles selecionados quando você verifica **serviços de informações da Internet** são suficientes.

Também será necessário instalar o ASP.NET 4,5 ou superior. Para instalá-lo, localize **serviços de informações da Internet-> serviços de World Wide Web > recursos de desenvolvimento de aplicativos**. Selecione uma versão de ASP.NET que seja maior ou igual a ASP.NET 4,5.

![Captura de tela da instalação do recurso ASP.NET](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>Etapa 2-instalar o Visual Studio 2017 e as ferramentas de desenvolvimento da Web 

[Instale o Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) se você ainda não o tiver instalado. Se você já tiver o Visual Studio 2017, verifique se as cargas de trabalho a seguir estão instaladas. Se as cargas de trabalho não estiverem presentes na instalação, acompanhe o uso do Instalador do Visual Studio (encontrado no menu Iniciar).  

Durante a instalação, selecione **ASP.net e desenvolvimento** para a Web e quaisquer outras cargas de trabalho em que você esteja interessado. 

Quando a instalação for concluída, inicie o Visual Studio e crie um novo projeto (**arquivo** -> **novo projeto**).

## <a name="step-3---build-a-web-app"></a>Etapa 3 – criar um aplicativo Web

Inicie o Visual Studio 2017 como **administrador** e crie um novo projeto de **aplicativo Web Visual C#**  com um modelo de projeto **vazio** . 

![Captura de tela da criação de um novo projeto Web](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>Etapa 4 – configurar o IIS com nosso aplicativo Web 

No Gerenciador de Soluções, clique com o botão direito do mouse no projeto raiz e selecione **Propriedades**.

Nas propriedades do aplicativo Web, selecione a guia **Web** . Na seção **servidores** , escolha **local IIS** no menu suspenso e clique em **criar diretório virtual**. 

![Captura de tela da guia da Web em Propriedades do projeto](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>Etapa 5 – adicionar um pacote de aplicativo a um aplicativo Web 

Adicione o pacote do aplicativo que você vai distribuir para o aplicativo Web. Você pode usar o pacote do aplicativo que faz parte dos [pacotes de projeto inicial](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages) fornecidos no GitHub se você não tiver um pacote de aplicativo disponível. O certificado (MySampleApp.cer) que o pacote usou também faz parte da amostra no GitHub. Você deve ter o certificado instalado em seu dispositivo antes de instalar o aplicativo (etapa 9).

No aplicativo Web do projeto inicial, uma nova pasta foi adicionada ao aplicativo Web chamado **pacotes** que contém os pacotes de aplicativos a serem distribuídos. Para criar a pasta no Visual Studio, clique com o botão direito do mouse no nó do projeto em Gerenciador de Soluções, selecione **adicionar** -> **nova pasta** e nomeie-o como **pacotes**. Para adicionar pacotes de aplicativos à pasta, clique com o botão direito do mouse na pasta **pacotes** e selecione **Adicionar** -> **Item existente...** e navegue até o local do pacote do aplicativo. 

![Captura de tela da adição de um pacote](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>Etapa 6 – criar uma página da Web

Este aplicativo Web de exemplo usa HTML simples. Você está livre para criar seu aplicativo Web conforme necessário de acordo com suas necessidades. 

Clique com o botão direito do mouse no projeto raiz do Gerenciador de soluções, selecione **adicionar** -> **novo item**e adicione uma nova **página HTML** da seção **da Web** .

Depois que a página HTML for criada, clique com o botão direito do mouse na página HTML na Gerenciador de Soluções e selecione **definir como página inicial**.  

Clique duas vezes no arquivo HTML para abri-lo na janela Editor de código. Neste tutorial, somente os elementos no necessário na página da Web para invocar o aplicativo instalador do aplicativo com êxito para instalar um aplicativo do Windows 10 serão usados. 

Inclua o código HTML a seguir em sua página da Web. A chave para invocar com êxito o instalador do aplicativo é usar o esquema personalizado que o instalador do aplicativo registra com o sistema operacional: `ms-appinstaller:?source=`. Consulte o exemplo de código abaixo para obter mais detalhes.

> [!NOTE]
> Verifique se o caminho da URL especificado após o esquema personalizado corresponde à URL do projeto na guia Web da solução VS.
 
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

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>Etapa 7-configurar o aplicativo Web para tipos de MIME do pacote de aplicativos

Abra o arquivo **Web. config** no Gerenciador de soluções e adicione as linhas a seguir no elemento `<configuration>`. 

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

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>Etapa 8 – adicionar a isenção de auto-retorno para o instalador do aplicativo

Devido ao isolamento de rede, os aplicativos do Windows 10, como o instalador do aplicativo, são restritos a usar endereços IP loopback como http://localhost/. Ao usar o servidor IIS local, o instalador de aplicativo deve ser adicionado à lista de isenção de auto-retorno. 

Para fazer isso, abra o **prompt de comando** como **administrador** e insira o seguinte:
```Command Line
CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

Para verificar se o aplicativo foi adicionado à lista de isenção, use o seguinte comando para exibir os aplicativos na lista de isenção de auto-retorno: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

Você deve encontrar `microsoft.desktopappinstaller_8wekyb3d8bbwe` na lista.

Depois que a validação local da instalação do aplicativo por meio do instalador do aplicativo for concluída, você poderá remover a isenção de auto-retorno adicionada nesta etapa:

```Command Line
CheckNetIsolation.exe LoopbackExempt -d -n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

## <a name="step-9---run-the-web-app"></a>Etapa 9 – executar o aplicativo Web 

Crie e execute o aplicativo Web clicando no botão Executar na faixa de faixas do VS, conforme mostrado na imagem abaixo:

![Captura de tela da execução do aplicativo Web no Visual Studio](images/run.png)

Uma página da Web será aberta em seu navegador:

![Captura de tela da instalação do aplicativo na página da Web](images/web-page.png)

Clique no link na página da Web para iniciar o aplicativo instalador do aplicativo e instalar o pacote de aplicativos do Windows 10.


## <a name="troubleshooting-issues"></a>Solucionando problemas

### <a name="not-sufficient-privilege"></a>Privilégio insuficiente 

Se a execução do aplicativo Web no Visual Studio exibir um erro como "você não tem privilégios suficientes para acessar sites do IIS em seu computador", será necessário executar o Visual Studio como administrador. Feche a instância atual do Visual Studio e abra-a novamente como administrador.

### <a name="set-start-page"></a>Definir página inicial 

Se a execução do aplicativo Web fizer com que o navegador seja carregado com um erro HTTP 403,14-proibido, isso ocorre porque o aplicativo Web não tem uma página inicial definida. Consulte a etapa 6 deste tutorial para saber como definir uma página inicial.
