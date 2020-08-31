---
title: Criar manualmente um arquivo do Instalador de Aplicativo
description: Este artigo descreve como instalar um conjunto relacionado por meio do instalador do aplicativo, incluindo como criar um arquivo *. AppInstaller que define o conjunto relacionado.
ms.date: 1/4/2018
ms.topic: article
keywords: windows 10, uwp, instalador de aplicativo, AppInstaller, sideload, conjunto relacionado, pacotes opcionais
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 9ecbaeb0e9dc85ddb8c3d4f9b6d8ed83caba9739
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89089814"
---
# <a name="create-an-app-installer-file-manually"></a>Criar manualmente um arquivo do Instalador de Aplicativo

Este artigo mostra como criar manualmente um arquivo do instalador de aplicativo que define um [conjunto relacionado](../package/optional-packages.md). Um conjunto relacionado não é uma entidade, mas em vez disso, uma combinação de um pacote principal e pacotes opcionais. 

Para poder instalar um conjunto relacionado como uma entidade, nós devemos ser capazes de especificar o pacote principal e o pacote opcional como um. Para fazer isso, será necessário criar um arquivo XML com uma extensão **. AppInstaller** para definir um conjunto relacionado. O instalador do aplicativo consome o arquivo **. AppInstaller** e permite que o usuário instale todos os pacotes definidos com um único clique. 

## <a name="app-installer-file-example"></a>Exemplo de arquivo do instalador do aplicativo

Antes de entrarmos em mais detalhes, aqui está um exemplo completo de arquivo msixbundle *. AppInstaller:

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix"
            ProcessorArchitecture="x64" />

    </OptionalPackages>
    
    <UpdateSettings>
    <OnLaunch HoursBetweenUpdateChecks="0"/>   
  </UpdateSettings>

</AppInstaller>
```

Durante a implantação, o arquivo do Instalador de Aplicativo é validado contra os pacotes de aplicativos referenciados no elemento `Uri`. Assim, o `Name`, `Publisher` e `Version` devem corresponder ao elemento [Pacote/Identidade](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) no manifesto do conjunto de aplicativo.

## <a name="how-to-create-an-app-installer-file"></a>Como criar um arquivo de Instalador de Aplicativo

Para distribuir seu conjunto relacionado como uma entidade, você deve criar um arquivo do Instalador de Aplicativo que contém os elementos que são exigidos pelo [Esquema appinstaller](/uwp/schemas/appinstallerschema/app-installer-file).

### <a name="step-1-create-the-appinstaller-file"></a>Etapa 1: Criar o arquivo *.appinstaller
Usando um editor de texto, crie um arquivo (que irá conter XML) e nomeie-o &lt;filename&gt;.appinstaller.

### <a name="step-2-add-the-basic-template"></a>Etapa 2: Adicionar o modelo básico
O modelo básico inclui as informações de arquivo do Instalador de Aplicativo.
```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >
</AppInstaller>
```

### <a name="step-3-add-the-main-package-information"></a>Etapa 3: Adicionar as informações de pacote principal

Se o pacote do aplicativo principal for um arquivo. msix ou. Appx, use `<MainPackage>` , conforme mostrado abaixo. Certifique-se de incluir o ProcessorArchitecture, pois ele é obrigatório para pacotes que não são de pacote.

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainPackage
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        ProcessorArchitecture="x64"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msix" />

</AppInstaller>
```

Se o pacote do aplicativo principal for um. msixbundle ou. appxbundle ou arquivo, use o `<MainBundle>` no lugar de, `<MainPackage>` conforme mostrado abaixo. Para os pacotes, o ProcessorArchitecture não é necessário.

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

</AppInstaller>
```

As informações no atributo `<MainBundle>` ou `<MainPackage>` devem corresponder ao elemento [Pacote/Identidade](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) no manifesto do lote de aplicativo ou manifesto do conjunto de aplicativo, respectivamente.

### <a name="step-4-add-the-optional-packages"></a>Etapa 4: Adicionar os pacotes opcionais
Semelhante ao atributo de conjunto de aplicativo principal, se o pacote opcional puder ser um conjunto de aplicativo ou um lote do aplicativo, o elemento filho dentro do atributo `<OptionalPackages>` deve ser `<Package>`ou `<Bundle>`, respectivamente. As informações nos elementos filho do pacote devem corresponder ao elemento identidade no manifesto do pacote ou lote.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x64"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix" />

    </OptionalPackages>

</AppInstaller>
```

### <a name="step-5-add-dependencies"></a>Etapa 5: Adicionar dependências
No elemento de dependências, você pode especificar os pacotes de estrutura necessários para o pacote principal ou para os pacotes opcionais.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

</AppInstaller>
```

### <a name="step-6-add-update-setting"></a>Etapa 6: Adicionar a configuração de atualização

O arquivo do Instalador de Aplicativo também pode especificar a configuração de atualização para que os conjuntos relacionados possam ser atualizados automaticamente quando um arquivo mais recente do Instalador de Aplicativo for publicado. **<UpdateSettings>** é um elemento opcional. Na  **<UpdateSettings>** opção OnLaunch especifica que as verificações de atualização devem ser feitas na inicialização do aplicativo e HoursBetweenUpdateChecks = "12" especifica que uma verificação de atualização deve ser feita a cada 12 horas. Se HoursBetweenUpdateChecks não for especificado, o intervalo padrão usado para verificar se há atualizações será de 24 horas. Tipos adicionais de atualizações, como atualizações em segundo plano, podem ser encontrados no [esquema](/uwp/schemas/appinstallerschema/element-update-settings)de configurações de atualização; Tipos adicionais de atualizações ao iniciar, como atualizações com um prompt, podem ser encontrados no [esquema](/uwp/schemas/appinstallerschema/element-onlaunch) OnLaunch

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

    <UpdateSettings>
        <OnLaunch HoursBetweenUpdateChecks="12" />
    </UpdateSettings>

</AppInstaller>
```

Para todos os detalhes sobre o esquema XML, consulte [Referência de arquivo do Instalador de Aplicativo](/uwp/schemas/appinstallerschema/app-installer-file).

> [!NOTE]
> O tipo de arquivo do instalador do aplicativo é novo no Windows 10, versão 1709 (a atualização dos criadores de outono do Windows 10). Não há suporte para a implantação de aplicativos do Windows 10 usando um arquivo do instalador de aplicativos em versões anteriores do Windows 10. O elemento **HoursBetweenUpdateChecks** está disponível a partir do Windows 10, versão 1803.