---
Description: Mostra como empacotar manualmente um aplicativo de área de trabalho do Windows (Win32, WPF e Windows Forms) para Windows 10.
title: Empacotar um aplicativo manualmente (ponte de desktop)
ms.date: 07/29/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e36cfe1a0ed1c16d2778d33599133d7bc9e06c1c
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68685376"
---
# <a name="package-a-desktop-app-manually"></a>Empacotar um aplicativo de área de trabalho manualmente

Este artigo mostra como empacotar seu aplicativo sem usar ferramentas como o Visual Studio ou a ferramenta de empacotamento MSIX.

Para empacotar manualmente seu aplicativo, crie um arquivo de manifesto de pacote e execute a ferramenta de linha de comando **MakeAppx. exe** para gerar um pacote de aplicativo do Windows.

Considere o empacotamento manual se você instalar seu aplicativo usando o comando xcopy ou se estiver familiarizado com as alterações que o instalador do aplicativo faz no sistema e quiser um controle mais granular sobre o processo.

Se não tiver certeza sobre quais alterações seu instalador faz no sistema, ou se você preferir usar ferramentas automatizadas para geral seu manifesto do pacote, considere qualquer uma [dessas](desktop-to-uwp-root.md#convert) opções.

> [!IMPORTANT]
> A capacidade de criar um pacote de aplicativo do Windows para seu aplicativo de desktop (também conhecido como ponte de desktop) foi introduzida no Windows 10, versão 1607, e ela só pode ser usada em projetos direcionados à atualização de aniversário do Windows 10 (10,0; Build 14393) ou uma versão posterior no Visual Studio.

## <a name="first-prepare-your-application"></a>Primeiro, prepare seu aplicativo

Examine este guia antes de começar a criar um pacote para seu aplicativo: [Prepare-se para empacotar um aplicativo de área de trabalho](desktop-to-uwp-prepare.md).

## <a name="create-a-package-manifest"></a>Criar um manifesto do pacote

Crie um arquivo, nomeie ele **appxmanifest.xml**, e, em seguida, adicione esse XML a ele.

É um modelo básico que contém os elementos e atributos que seu pacote precisa. Vamos adicionar valores a eles na próxima seção.

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

## <a name="fill-in-the-package-level-elements-of-your-file"></a>Preencha os elementos de nível de pacote do seu arquivo

Preencha este modelo com informações que descrevem o seu pacote.

### <a name="identity-information"></a>Informações de identidade

Aqui está um exemplo de elemento **Identidade** com texto de espaço reservado para os atributos. Você pode definir o atributo ``ProcessorArchitecture`` para ``x64`` ou ``x86``.

```XML
<Identity Name="MyCompany.MySuite.MyApp"
          Version="1.0.0.0"
          Publisher="CN=MyCompany, O=MyCompany, L=MyCity, S=MyState, C=MyCountry"
                ProcessorArchitecture="x64">
```
> [!NOTE]
> Se você reservou o nome do aplicativo no Microsoft Store, poderá obter o nome e o Publicador usando o [Partner Center](https://partner.microsoft.com/dashboard). Se você planeja Sideload seu aplicativo em outros sistemas, você pode fornecer seus próprios nomes, desde que o nome do Publicador que você escolher corresponda ao nome no certificado que você usa para assinar seu aplicativo.

### <a name="properties"></a>Properties

O elemento [Propriedades](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-properties) possui 3 elementos filho necessários. Aqui está um nó exemplo **Propriedades** com texto de espaço reservado para os elementos. O **DisplayName** é o nome do aplicativo que você reserva no repositório, para aplicativos que são carregados no repositório.

```XML
<Properties>
  <DisplayName>MyApp</DisplayName>
  <PublisherDisplayName>MyCompany</PublisherDisplayName>
  <Logo>images\icon.png</Logo>
</Properties>
```

### <a name="resources"></a>Recursos

Aqui está um nó exemplo [Recursos](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-resources).

```XML
<Resources>
  <Resource Language="en-us" />
</Resources>
```
### <a name="dependencies"></a>Dependências

Para aplicativos de área de trabalho para os quais você cria um pacote ``Name`` , sempre ``Windows.Desktop``defina o atributo como.

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>Capacidades
Para aplicativos de área de trabalho para os quais você cria um pacote, você precisará adicionar o ``runFullTrust`` recurso.

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>Preencha os elementos de nível de aplicativo

Preencha este modelo com informações que descrevem o seu aplicativo.

### <a name="application-element"></a>Elemento do aplicativo

Para aplicativos de área de trabalho para os quais você cria ``EntryPoint`` um pacote, o atributo do elemento ``Windows.FullTrustApplication``Application é sempre.

```XML
<Applications>
  <Application Id="MyApp"     
        Executable="MyApp.exe" EntryPoint="Windows.FullTrustApplication">
   </Application>
</Applications>
```

### <a name="visual-elements"></a>Elementos visuais

Aqui está um nó exemplo [VisualElements](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements).

```XML
<uap:VisualElements
    BackgroundColor="#464646"
    DisplayName="My App"
    Square150x150Logo="images\icon.png"
    Square44x44Logo="images\small_icon.png"
    Description="A useful description" />
```
<a id="target-based-assets" />

## <a name="optional-add-target-based-unplated-assets"></a>(Opcional) Adicionar ativos não incluídos no destino

Os ativos baseados no destino são para ícones e blocos que aparecem na barra de tarefas do Windows, na visão de tarefas, em ALT+TAB, no Assistente de Ajuste e no canto inferior direito dos blocos em Iniciar. Você pode ler mais sobre eles [aqui](https://docs.microsoft.com/windows/uwp/design/style/app-icons-and-logos#unplated-assets).

1. Obtenha as imagens de 44x44 corretas e então copie-as na pasta que contém suas imagens (i.e., Ativos).

2. Para cada imagem de 44 x 44, crie uma cópia na mesma pasta e acrescente **.targetsize-44_altform-unplated** ao nome do arquivo. Você deve ter duas cópias de cada ícone, cada uma nomeada de uma maneira específica. Por exemplo, após concluir o processo, sua pasta de ativos pode conter **MYAPP_44x44.png** e **MYAPP_44x44.targetsize-44_altform-unplated.png**.

   > [!NOTE]
   > Neste exemplo, o ícone chamado **MYAPP_44x44.png** é o ícone que você irá referenciar no atributo logo ``Square44x44Logo`` do seu pacote do aplicativo do Windows.

3.  No pacote do aplicativo do Windows, defina a ``BackgroundColor`` para todo ícone que fizer transparente.

4. Continue até a próxima subseção para gerar um novo arquivo de Índice de recurso do pacote.

<a id="make-pri" />

### <a name="generate-a-package-resource-index-pri-file"></a>Gerar um arquivo PRI (Índice de Recurso do Pacote)

Se você criar ativos baseados em destino, conforme descrito na seção acima, ou modificar qualquer um dos ativos visuais de seu aplicativo depois de criar o pacote, terá que gerar um novo arquivo PRI.

1.  Abra um **Prompt de comando de desenvolvedor para o VS 2017**.

    ![prompt de comando de desenvolvedor](images/developer-command-prompt.png)

2.  Altere o diretório para a pasta raiz do pacote e crie um arquivo priconfig.xml executando o comando ``makepri createconfig /cf priconfig.xml /dq en-US``.

5.  Crie o(s) arquivo(s) resources.pri usando o comando ``makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml``.

    Por exemplo, o comando para seu aplicativo pode ter a seguinte aparência ``makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``:.

6.  Empacote seu pacote do aplicativo do Windows usando as instruções da próxima etapa.

<a id="make-appx" />

## <a name="generate-a-windows-app-package"></a>Gerar um pacote do aplicativo do Windows

Use o **MakeAppx.exe** para gerar um pacote do aplicativo do Windows para seu projeto. Ele é fornecido com o SDK do Windows 10 e, se você tiver o Visual Studio instalado, poderá ser facilmente acessado por meio do Prompt de Comando do Desenvolvedor para a sua versão do Visual Studio.

Consulte [Criar um pacote do aplicativo com a ferramenta MakeAppx.exe](../package/create-app-package-with-makeappx-tool.md)

## <a name="run-the-packaged-app"></a>Execute o app empacotado

Você pode executar seu aplicativo para testá-lo localmente sem ter que obter um certificado e conectá-lo. Basta executar este cmdlet do PowerShell:

```Add-AppxPackage –Register AppxManifest.xml```

Para atualizar os arquivos .exe ou .dll do seu aplicativo, substitua os arquivos existentes em seu pacote pelos novos, aumente o número de versão em AppxManifest.xml e, em seguida, execute o comando acima novamente.

> [!NOTE]
> Um aplicativo empacotado sempre é executado como um usuário interativo, e qualquer unidade na qual você instala o aplicativo empacotado deve ser formatada para o formato NTFS.

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora esses [tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode perguntar [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fornecer comentários ou fazer sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Percorrer código/localizar e corrigir problemas**

Consulte [Executar, depurar e testar um aplicativo de área de trabalho empacotado](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-debug)

**Assinar seu aplicativo e, em seguida, distribuí-lo**

Consulte [distribuir um aplicativo de área de trabalho empacotado](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-distribute)
