---
title: Passar parâmetros de instalação para seu aplicativo por meio do Instalador de Aplicativo
description: Descreve como passar parâmetros de instalação para um aplicativo por meio de instalador de aplicativo e ativação de protocolo.
author: Huios
ms.date: 12/17/2020
ms.topic: article
keywords: Windows 10, UWP, pacote do aplicativo, atualização do aplicativo, MSIX, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 5673881ba9acb1ae9420a02bb50c356c0e92f2aa
ms.sourcegitcommit: 059f215a0804adeeefeaaa09b376684caa4382eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98768841"
---
# <a name="passing-installation-parameters-to-your-app-via-app-installer"></a>Passar parâmetros de instalação para seu aplicativo por meio do Instalador de Aplicativo

Ao distribuir seu aplicativo usando o MSIX, você pode configurar seu aplicativo de modo que os parâmetros de cadeia de caracteres de consulta definidos no URI de download/instalação sejam passados para seu aplicativo quando ele for iniciado, depois que um usuário clicar no URI de download/instalação. Isso funcionará se for a primeira vez que um usuário estiver instalando o aplicativo ou se o aplicativo tiver sido instalado anteriormente. Este artigo mostra como configurar seu aplicativo MSIX empacotado e seu URI de download/instalação para aproveitar essa funcionalidade. Isso pode ser útil se você quiser controlar ou lidar com instalações diferentes com base na origem, no tipo de download etc, e funcionará para downloads da Web e quaisquer outros casos em que um usuário clicar no URI, por exemplo, de uma campanha de email. Para obter mais detalhes, Confira esta [postagem no blog](https://techcommunity.microsoft.com/t5/windows-dev-appconsult/passing-installation-parameters-to-a-windows-application-with/ba-p/1719829).

## <a name="configure-your-application-for-protocol-activation"></a>Configurar seu aplicativo para ativação de protocolo

A primeira coisa a fazer é registrar seu aplicativo para que ele seja iniciado usando um [protocolo personalizado](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-your-application-in-different-ways) que você definir. Quando esse protocolo é invocado, seu aplicativo é iniciado e qualquer prameters especificada no URI é passada para os argumentos do evento de ativação do aplicativo quando ele é iniciado. Você pode registrar o protocolo adicionando uma entrada de extensão de protocolo no nó extensões de aplicativo no arquivo de appxmanifest.xml do MSIX:

```xml
<Application>
...
   <Extensions>
     <uap:Extension Category="windows.protocol">
        <uap:Protocol Name="my-custom-protocol"/>
     </uap:Extension>
   </Extensions>
  
...
</Application>
```

Se você estiver usando o [projeto de empacotamento do Windows](../desktop/desktop-to-uwp-packaging-dot-net.md), também poderá definir um protocolo personalizado usando o editor de manifesto padrão clicando duas vezes no arquivo _Package. appxmanifest_ , navegando até a guia _declarações_ e selecionando _protocolo_ em _declarações disponíveis_:

![Declaração de protocolo em Package. appxmanifest](images/custom-protocol.PNG)

##  <a name="write-code-to-handle-parameters-when-your-app-is-launched-after-installation"></a>Escreva o código para lidar com parâmetros quando seu aplicativo é iniciado após a instalação

Você precisará implementar o código em seu aplicativo para manipular os parâmetros de instalação que serão passados para seu aplicativo quando ele for iniciado. O código de exemplo a seguir usa o método [AppInstance. Getactivatedeventargs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs?view=winrt-19041) para determinar o tipo de ativação usado para criar uma instância de um aplicativo (você também pode manipular os parâmetros usando um método diferente). Quando seu aplicativo é iniciado/ativado com parâmetros de consulta Stinger de um URI de instalação (consulte a definição na próxima seção), o tipo de ativação será uma ativação de protocolo, conforme definido pelo seu protocolo personalizado declarado em seu appxmanifest.xml e o URI de download/instalação. Os args de evento de ativação serão do tipo [ProtocolActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.protocolactivatedeventargs?view=winrt-19041) e isso é o que o código abaixo usa:

```csharp

using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;

public static void Main(string[] cmdArgs)
{
            
    var activationArgs = AppInstance.GetActivatedEventArgs();
    switch (activationArgs.Kind)
    {
        //Install parameters will be passed in during a protocol activation
        case ActivationKind.Protocol:
        HandleProtocolActivation(activationArgs as ProtocolActivatedEventArgs);
        break;
        case ActivationKind.Launch:
        //Regular launch activation type
        HandleLaunch(activationArgs as LaunchActivatedEventArgs);
        break;
        default:
        break;
     }       
    

     static void HandleProtocolActivation(ProtocolActivatedEventArgs args)
     {

         if (args.Uri != null)
        {
            //Handle the installation parameters in the protocol uri
            handleInstallParameter(args.Uri.ToString());

        }
            
}
```

## <a name="add-your-custom-activation-protocol-and-parameters-to-the-installation-uri"></a>Adicionar seu protocolo de ativação e parâmetros personalizados ao URI de instalação

Depois que seu aplicativo estiver configurado para lidar com os parâmetros de instalação, você poderá personalizar o URI de instalação/download do aplicativo para conter parâmetros definidos com exclusividade que serão passados para seu aplicativo na inicialização, depois que um usuário clicar no URI. O URI deve conter:

1. O protocolo [MS-AppInstaller](./installing-windows10-apps-web.md#protocol-activation-scheme) que invoca o instalador do aplicativo.
2. O parâmetro exclusivo **activationUri** que aponta para o protocolo personalizado do seu aplicativo e os parâmetros de instalação que você deseja passar para seu aplicativo quando ele é iniciado.
3. O protocolo personalizado do seu aplicativo e o parâmetro e seu valor.

Nos URIs de exemplo abaixo, defini um protocolo personalizado _meu-Custom-Protocol_, um parâmetro _My-Parameter_ e dado a ele o valor _My-param-value_. Quando o aplicativo for iniciado depois que um usuário clicar no URI, ele receberá a parte da cadeia de caracteres de consulta do URI após **activationUri**, nesse caso, que será _meu-Custom-Protocol:? meu-Parameter = meu-param-value_.

```html
ms-appinstaller:?source=https://contoso.com/myapp.appinstaller&activationUri=my-custom-protocol:?my-parameter=my-param-value
```
```html
ms-appinstaller:?source=https://contos.com/myapp.msix&activationUri=my-custom-protocol:?my-parameter=my-param-value
```