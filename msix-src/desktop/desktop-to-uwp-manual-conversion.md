---
description: Mostra como empacotar manualmente um aplicativo de área de trabalho do Windows (como Win32, WPF e Windows Forms) para Windows 10.
title: Empacotar um aplicativo manualmente (Ponte de Desktop)
ms.date: 07/29/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0abfa56857bc417df1381ec56ef3627ac17b7e1c
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090044"
---
# <a name="generating-msix-package-components"></a>Geração de componentes do pacote MSIX

Este artigo mostra como gerar componentes do pacote MSIX para empacotar o aplicativo usando ferramentas de linha de comando (sem usar o Visual Studio ou a Ferramenta de Empacotamento MSIX).

Para empacotar o aplicativo manualmente, é necessário criar um arquivo de manifesto do pacote, adicionar os componentes do pacote e executar a ferramenta de linha de comando **MakeAppx.exe** para gerar um pacote MSIX.

## <a name="first-prepare-to-package"></a>Primeiro, prepare para empacotar

Caso ainda não tenha feito isso, revise esta seção sobre [o que você precisa saber antes de empacotar o aplicativo](../desktop/before-packaging-overview.md).

## <a name="create-a-package-manifest"></a>Crie um manifesto do pacote

Crie um arquivo, chame-o de **appxmanifest.xml**, e, em seguida, adicione esse XML a ele.

É um modelo básico que contém os elementos e atributos de que seu pacote precisa. Vamos adicionar valores a eles na próxima seção.

```XML
<?xml version="1.0" encoding="utf-8"?>
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
  <Identity Name="" Version="" Publisher="" ProcessorArchitecture="" />
    <Properties>
       <DisplayName></DisplayName>
       <PublisherDisplayName></PublisherDisplayName>
             <Description></Description>
      <Logo></Logo>
    </Properties>
    <Resources>
      <Resource Language="" />
    </Resources>
      <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="" MaxVersionTested="" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
    <Applications>
      <Application Id="" Executable="" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements DisplayName="" Description=""   Square150x150Logo=""
                   Square44x44Logo=""   BackgroundColor="" />
      </Application>
     </Applications>
  </Package>
```

## <a name="fill-in-the-package-level-elements-of-your-file"></a>Preencha os elementos em nível de pacote do arquivo

Preencha este modelo com informações que descrevem o pacote.

### <a name="identity-information"></a>Informações de identidade

Este é um exemplo do elemento **Identity** com texto de espaço reservado para os atributos. Você pode definir o atributo ``ProcessorArchitecture`` como ``x64`` ou ``x86``.

```XML
<Identity Name="MyCompany.MySuite.MyApp"
          Version="1.0.0.0"
          Publisher="CN=MyCompany, O=MyCompany, L=MyCity, S=MyState, C=MyCountry"
                ProcessorArchitecture="x64">
```
> [!NOTE]
> Se tiver reservado o nome do aplicativo na Microsoft Store, será possível obter o Nome e o Editor usando o [Partner Center](https://partner.microsoft.com/dashboard). Se pretende fazer o sideload do aplicativo para outros sistemas, é possível fornecer seus próprios nomes para eles, desde que o nome de editor que você escolher corresponda ao nome no certificado usado para assinar o aplicativo.

### <a name="properties"></a>Propriedades

O elemento [Properties](/uwp/schemas/appxpackage/appxmanifestschema/element-properties) tem 3 elementos filhos necessários. Este é um exemplo do nó **Propriedades** com texto de espaço reservado para os elementos. O **DisplayName** é o nome do aplicativo que você reserva na Microsoft Store para aplicativos que são carregados na Microsoft Store.

```XML
<Properties>
  <DisplayName>MyApp</DisplayName>
  <PublisherDisplayName>MyCompany</PublisherDisplayName>
  <Logo>images\icon.png</Logo>
</Properties>
```

### <a name="resources"></a>Recursos

Este é um exemplo do nó [Recursos](/uwp/schemas/appxpackage/appxmanifestschema/element-resources).

```XML
<Resources>
  <Resource Language="en-us" />
</Resources>
```
### <a name="dependencies"></a>Dependências

Nos aplicativos da área de trabalho para os quais você cria um pacote, sempre defina o atributo ``Name`` como ``Windows.Desktop``.

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>Capacidades
Nos aplicativos da área de trabalho para os quais você cria um pacote, será preciso adicionar o recurso ``runFullTrust``.

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>Preencha os elementos no nível do aplicativo

Preencha este modelo com informações que descrevem o aplicativo.

### <a name="application-element"></a>Elemento Application

Nos aplicativos de área de trabalho para os quais você cria um pacote, o atributo ``EntryPoint`` do elemento Application é sempre ``Windows.FullTrustApplication``.

```XML
<Applications>
  <Application Id="MyApp"     
        Executable="MyApp.exe" EntryPoint="Windows.FullTrustApplication">
   </Application>
</Applications>
```

### <a name="visual-elements"></a>Elementos visuais

Este é um exemplo do nó [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements).

```XML
<uap:VisualElements
    BackgroundColor="#464646"
    DisplayName="My App"
    Square150x150Logo="images\icon.png"
    Square44x44Logo="images\small_icon.png"
    Description="A useful description" />
```
<a id="target-based-assets"></a>

## <a name="optional-add-target-based-unplated-assets"></a>(Opcional) Adicione ativos não incluídos no destino

Os ativos baseados no destino são para ícones e blocos que aparecem na barra de tarefas do Windows, na visão de tarefas, em ALT+TAB, no Assistente de Ajuste e no canto inferior direito dos blocos em Iniciar. Você pode ler mais sobre isso [aqui](/windows/uwp/design/style/app-icons-and-logos#unplated-assets).

1. Obtenha as imagens 44x44 corretas e copie-as para a pasta que contém as imagens (ou seja, Ativos).

2. Para cada imagem de 44 x 44, crie uma cópia na mesma pasta e acrescente **.targetsize-44_altform-unplated** ao nome do arquivo. Você deve ter duas cópias de cada ícone, cada uma nomeada de uma maneira específica. Por exemplo, depois de concluir o processo, sua pasta de ativos poderá conter **MYAPP_44x44.png** e **MYAPP_44x44.targetsize-44_altform-unplated.png**.

   > [!NOTE]
   > Neste exemplo, o ícone chamado **MYAPP_44x44.png** é o ícone ao qual você vai fazer referência no atributo ``Square44x44Logo`` do logotipo do pacote MSIX.

3. No arquivo de manifesto, defina o ``BackgroundColor`` para cada ícone que está tornando transparente.

4. Continue até a próxima subseção para gerar um novo arquivo de índice de recurso do pacote.

<a id="make-pri"></a>

### <a name="generate-a-package-resource-index-pri-file-using-makepri"></a>Gerar um arquivo PRI (índice de recurso do pacote) usando MakePri

Se você criar ativos com base no destino como descrito na seção acima, ou se modificar qualquer um dos ativos visuais do aplicativo depois de criar o pacote, será necessário gerar um novo arquivo PRI.

Com base no caminho da instalação do SDK, é aqui que o **MakePri.exe** fica localizado no computador com Windows 10:
- x86: C:\Arquivos de Programas (x86)\Windows Kits\10\bin\\&lt;número do build&gt;\x86\makepri.exe
- x64: C:\Arquivos de Programas (x86)\Windows Kits\10\bin\\&lt;número do build&gt;\x64\makepri.exe

Não há nenhuma versão ARM dessa ferramenta.

1.  Abra um Prompt de Comando ou uma janela do PowerShell.

2.  Altere o diretório para a pasta raiz do pacote e crie um arquivo priconfig.xml executando o comando ``<path>\makepri.exe createconfig /cf priconfig.xml /dq en-US``.

5.  Crie os arquivos resources.pri usando o comando ``<path>\makepri.exe new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml``.

    Por exemplo, o comando do aplicativo pode ter essa aparência: ``<path>\makepri.exe new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``.

6.  Empacote o aplicativo usando as instruções da próxima etapa.

<a id="make-appx"></a>

## <a name="test-your-application-before-packaging"></a>Teste o aplicativo antes de empacotar

Você pode implantar o aplicativo não empacotado e testá-lo antes de empacotar ou assinar. Para isso, execute o cmdlet abaixo de uma janela do PowerShell. Assegure-se de passar o arquivo de manifesto do aplicativo localizado na raiz do diretório com todos os outros componentes de pacote:

```Add-AppxPackage –Register AppxManifest.xml```

Depois de fazer isso, o aplicativo deverá ser implantado no sistema e você poderá testá-lo para assegurar que tudo esteja funcionando antes de empacotar. Para atualizar os arquivos .exe ou .dll do aplicativo, substitua os arquivos existentes no pacote pelos novos, aumente o número da versão no AppxManifest.xml e execute o comando acima novamente.

## <a name="package-your-components-into-an-msix"></a>Empacote os componentes em um MSIX

A próxima etapa é usar o **MakeAppx.exe** para gerar um pacote MSIX para o aplicativo. O Makeappx.exe está incluído com o SDK do Windows 10 e, caso você tenha o Visual Studio instalado, ele poderá ser acessado com facilidade pelo Prompt de Comando do Desenvolvedor para Visual Studio.

Consulte [Criar um pacote ou grupo MSIX com a ferramenta MakeAppx.exe](../package/create-app-package-with-makeappx-tool.md)



> [!NOTE]
> Um aplicativo empacotado sempre é executado como um usuário interativo e qualquer unidade na qual você instale seu aplicativo empacotado deve estar formatada em NTFS.
