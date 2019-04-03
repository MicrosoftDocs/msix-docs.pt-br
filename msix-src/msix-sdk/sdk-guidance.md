---
title: Diretrizes para desenvolvedores ao destino MSIX SDK
description: Diretrizes para desenvolvedores que estão usando o SDK para pacotes de MSIX pack para uso em plataforma cruzada
author: mcleanbyron
ms.author: mcleans
ms.date: 12/13/2018
ms.topic: article
ms.custom: RS5
ms.openlocfilehash: 2c6134aad4f1da8da4e835fa2e195861bd7a461a
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900638"
---
# <a name="use-the-msix-sdk-to-build-a-package-for-cross-platform-use"></a>Use o SDK de MSIX para compilar um pacote para uso de plataforma cruzada

O SDK MSIX oferece aos desenvolvedores uma maneira universal para distribuir o conteúdo do pacote para os dispositivos de cliente, independentemente da plataforma do sistema operacional no dispositivo cliente. Isso permite que os desenvolvedores empacotar seu conteúdo de aplicativo em vez de ter uma vez para o pacote para cada plataforma.

Para tirar proveito do SDK MSIX e a capacidade de distribuir o conteúdo do pacote para várias plataformas, fornecemos uma maneira de especificar as plataformas de destino onde você deseja que seus pacotes para extração. Isso significa que você pode garantir que o conteúdo do pacote está sendo extraído do pacote de apenas conforme desejado.

A tabela a seguir mostra as famílias de dispositivo de destino para declarar no manifesto.

<table class="tg">
  <tr>
    <th class="tg-yw4l">Plataforma</th>
    <th class="tg-yw4l">Família</th>
    <th class="tg-yw4l" colspan="3">Família do dispositivo de destino</th>
    <th class="tg-yw4l">Observações</th>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="6">Windows 10</td>
    <td class="tg-yw4l">Phone</td>
    <td class="tg-031e" rowspan="24"><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>Platform.All<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br></td>
    <td class="tg-baqh" rowspan="6">Windows.Universal</td>
    <td class="tg-yw4l">Windows.Mobile</td>
    <td class="tg-yw4l">Dispositivos móveis</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Área de Trabalho</td>
    <td class="tg-yw4l">Windows.Desktop</td>
    <td class="tg-yw4l">Computador</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Xbox</td>
    <td class="tg-yw4l">Windows.Xbox</td>
    <td class="tg-yw4l">Console do Xbox</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Surface Hub</td>
    <td class="tg-yw4l">Windows.Team</td>
    <td class="tg-yw4l">Dispositivos com tela grande Win 10</td>
  </tr>
  <tr>
    <td class="tg-yw4l">HoloLens</td>
    <td class="tg-yw4l">Windows.Holographic</td>
    <td class="tg-yw4l">Headset VR/AR</td>
  </tr>
  <tr>
    <td class="tg-yw4l">IoT</td>
    <td class="tg-yw4l">Windows.IoT</td>
    <td class="tg-yw4l">Dispositivos IoT</td>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="4">iOS</td>
    <td class="tg-yw4l">Phone</td>
    <td class="tg-yw4l" rowspan="4">Apple.Ios.All</td>
    <td class="tg-yw4l">Apple.Ios.Phone</td>
    <td class="tg-yw4l">iPhone, Touch</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Tablet</td>
    <td class="tg-yw4l">Apple.Ios.Tablet</td>
    <td class="tg-yw4l">iPad mini, iPad, iPad Pro</td>
  </tr>
  <tr>
    <td class="tg-yw4l">TV</td>
    <td class="tg-yw4l">Apple.Ios.TV</td>
    <td class="tg-yw4l">Apple TV</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Assistir</td>
    <td class="tg-yw4l">Apple.Ios.Watch</td>
    <td class="tg-yw4l">iWatch</td>
  </tr>
  <tr>
    <td class="tg-yw4l">MacOS</td>
    <td class="tg-yw4l">Área de Trabalho</td>
    <td class="tg-baqh" colspan="2">Apple.MacOS.All</td>
    <td class="tg-yw4l">MacBook Pro, iMac MacBook ar, Mac Mini,</td>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="5">Android</td>
    <td class="tg-yw4l">Phone</td>
    <td class="tg-yw4l" rowspan="5">Google.Android.All</td>
    <td class="tg-yw4l">Google.Android.Phone</td>
    <td class="tg-yw4l">Dispositivos móveis que se destinam a qualquer versão do Android</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Tablet</td>
    <td class="tg-yw4l">Google.Android.Tablet</td>
    <td class="tg-yw4l">Tablets Android</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Área de Trabalho</td>
    <td class="tg-yw4l">Google.Android.Desktop</td>
    <td class="tg-yw4l">Chromebooks</td>
  </tr>
  <tr>
    <td class="tg-yw4l">TV</td>
    <td class="tg-yw4l">Google.Android.TV</td>
    <td class="tg-yw4l">Dispositivos com tela grande Android</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Assistir</td>
    <td class="tg-yw4l">Google.Android.Watch</td>
    <td class="tg-yw4l">Dispositivos de engrenagem do Google</td>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="2">Windows</td>
    <td class="tg-yw4l">7</td>
    <td class="tg-baqh" colspan="2">Windows7.Desktop</td>
    <td class="tg-yw4l">Dispositivos do Windows 7</td>
  </tr>
  <tr>
    <td class="tg-yw4l">8</td>
    <td class="tg-baqh" colspan="2">Windows8.Desktop</td>
    <td class="tg-yw4l">Dispositivos do Windows 8/8.1</td>
  </tr>
  <tr>
    <td class="tg-yw4l" rowspan="5">Web</td>
    <td class="tg-yw4l">Microsoft</td>
    <td class="tg-yw4l" rowspan="5">Web.All</td>
    <td class="tg-yw4l">Web.Edge.All</td>
    <td class="tg-yw4l">Aplicativos web de mecanismo do Edge</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Android</td>
    <td class="tg-yw4l">Web.Blink.All</td>
    <td class="tg-yw4l">Aplicativos web do mecanismo de piscar</td>
  </tr>
    <tr>
    <td class="tg-yw4l">Cromado</td>
    <td class="tg-yw4l">Web.Chromium.All</td>
    <td class="tg-yw4l">Aplicativos do Chrome web mecanismo</td>
  </tr>
  <tr>
    <td class="tg-yw4l">iOS</td>
    <td class="tg-yw4l">Web.Webkit.All</td>
    <td class="tg-yw4l">Aplicativos web de mecanismo do WebKit</td>
  </tr>
  <tr>
    <td class="tg-yw4l">MacOS</td>
    <td class="tg-yw4l">Web.Safari.All</td>
    <td class="tg-yw4l">Aplicativos web de mecanismo do Safari</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Linux</td>
    <td class="tg-yw4l">Todas</td>
    <td class="tg-baqh" colspan="2">Linux.All</td>
    <td class="tg-yw4l">Todas as distribuições do Linux</td>
  </tr>
</table>

O arquivo de manifesto do pacote do aplicativo, você precisará incluir a família do dispositivo de destino apropriado se você deseja que o conteúdo do pacote a ser extraído apenas em dispositivos e plataformas específicas. Se você gosta de bulid o pacote de tal forma que ele é compatível com todos os tipos de plataforma e dispositivo, escolha **Platform.All** como a família do dispositivo de destino. Da mesma forma, se desejar que o pacote a ser só tem suporte em aplicativos web, escolha **Web.All**.

## <a name="sample-manifest-file-appxmanifestxml"></a>Arquivo de exemplo de manifesto (appxmanifest. xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
         IgnorableNamespaces="mp uap uap3">

  <Identity Name="BestAppExtension"
            Publisher="CN=awesomepublisher"
            Version="1.0.0.0" />

  <mp:PhoneIdentity PhoneProductId="56a6ecda-c215-4864-b097-447edd1f49fe" PhonePublisherId="00000000-0000-0000-0000-000000000000"/>

  <Properties>
    <DisplayName>Best App Extension</DisplayName>
    <PublisherDisplayName>Awesome Publisher</PublisherDisplayName>
    <Description>This is an extension package to my app</Description>
    <Logo>Assets\StoreLogo.png</Logo>
  </Properties>

  <Resources>
    <Resource Language="x-generate"/>
  </Resources>

  <Dependencies>
    <TargetDeviceFamily Name="Platform.All" MinVersion="0.0.0.0" MaxVersionTested="0.0.0.0"/>
  </Dependencies>

  <Applications>
    <Application Id="App">
      <uap:VisualElements
          DisplayName="Best App Extension"
          Description="This is the best app extension"
          BackgroundColor="white"
          Square150x150Logo="images\squareTile-sdk.png"
          Square44x44Logo="images\smallTile-sdk.png"
          AppListEntry="none">
      </uap:VisualElements>

      <Extensions>
        <uap3:Extension Category="Windows.appExtension">
          <uap3:AppExtension Name="add-in-contract" Id="add-in" PublicFolder="Public" DisplayName="Sample Add-in" Description="This is a sample add-in">
            <uap3:Properties>
               <!--Free form space-->
            </uap3:Properties>
          </uap3:AppExtension>
        </uap3:Extension>
      </Extensions>

    </Application>
  </Applications>
</Package>
```

## <a name="platform-version"></a>Versão da plataforma
No acima exemplo arquivo de manifesto, juntamente com o nome da plataforma, também há parâmetros para especificar o **MinVersion** e **MaxVersionTested** esses parâmetros são usados em plataformas Windows 10. No Windows 10, o pacote será implantado apenas em versões de sistema operacional do Windows 10 maiores que o MinVersion. Em outras plataformas não Windows 10, os parâmetros MinVersion e MaxVersionTested não são usados para tornar a declaração de extrair o conteúdo do pacote ou não.

Se você quiser usar o pacote para todas as plataformas (Windows 10 e Windows 10), é recomendável que você use os parâmetros MinVersion e MaxVersionTested para especificar as versões de sistema operacional do Windows 10 onde você deseja que seu aplicativo funcione. Portanto, seu manifesto **dependências** seção teria esta aparência:
```xml
  <Dependencies>
    <TargetDeviceFamily Name="Platform.All" MinVersion="0.0.0.0" MaxVersionTested="0.0.0.0"/>
    <TargetDeviceFamily Name="Windows.Universal" MinVersion="10.0.14393.0" MaxVersionTested="10.0.16294.0"/>
  </Dependencies>
```

**MinVersion** e **MaxVersionTested** são campos obrigatórios no manifesto e eles precisam estar em conformidade com o notation(#.#.#.#) quad. Se você estiver usando apenas o empacotamento MSIX SDK para apenas plataformas do Windows 10, você pode simplesmente usar '0.0.0.0' como o **MinVersion** e **MaxVersionTested** como as versões.

## <a name="how-to-effectively-use-the-same-package-on-all-platforms-windows-10-and-non-windows-10"></a>Como usar efetivamente o mesmo pacote em todas as plataformas (Windows 10 e Windows 10)

Para aproveitar ao máximo o SDK de empacotamento MSIX, você precisa compilar o pacote de forma que será implantado como um pacote do aplicativo no Windows 10 e ao mesmo tempo com suporte em outras plataformas. No Windows 10, você pode criar o pacote como um *extensão do aplicativo*. Para obter mais informações sobre extensões de aplicativo e como eles podem ajudar a tornar seu aplicativo extensível, consulte o [Introdução às extensões de aplicativo](https://blogs.msdn.microsoft.com/appinstaller/2017/05/01/introduction-to-app-extensions/) postagem de blog.

O exemplo de arquivo de manifesto mostrado anteriormente neste artigo, você observará uma **propriedades** elemento dentro de **AppExtension** elemento. Não há nenhuma validação executada nesta seção do arquivo de manifesto. Isso permite que os desenvolvedores especifiquem os metadados necessários entre o aplicativo host para o cliente e de extensão.
