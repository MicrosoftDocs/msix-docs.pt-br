---
title: Criar manualmente um arquivo do Instalador de Aplicativo
description: Este artigo descreve como instalar um conjunto relacionado por meio do instalador do aplicativo. Também explicaremos as etapas para construir um arquivo *.appinstaller que definirão o conjunto relacionado.
ms.date: 1/4/2018
ms.topic: article
keywords: windows 10, uwp, instalador de aplicativo, AppInstaller, sideload, conjunto relacionado, pacotes opcionais
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 61b98b924015f021950c5f6a022d383f940443c8
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828482"
---
# <a name="create-an-app-installer-file-manually"></a>Criar manualmente um arquivo do Instalador de Aplicativo

Este artigo mostra como criar manualmente um arquivo do instalador do aplicativo que define uma [relacionados ao conjunto](install-related-set.md). Um conjunto relacionado não é uma entidade, mas em vez disso, uma combinação de um pacote principal e pacotes opcionais. 

Para poder instalar um conjunto relacionado como uma entidade, nós devemos ser capazes de especificar o pacote principal e o pacote opcional como um. Para fazer isso, precisamos criar um arquivo XML com um **appinstaller** extensão para definir um conjunto relacionado. O instalador de aplicativo consome os **appinstaller** de arquivo e permite que o usuário instale todos os pacotes definidos com um único clique. 

## <a name="app-installer-file-example"></a>Exemplo de arquivo do instalador de aplicativo

Antes de entrarmos em mais detalhes, aqui está um arquivo de *.appinstaller msixbundle exemplo completo:

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

Durante a implantação, o arquivo do Instalador de Aplicativo é validado contra os pacotes de aplicativos referenciados no elemento `Uri`. Assim, o `Name`, `Publisher` e `Version` devem corresponder ao elemento [Pacote/Identidade](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) no manifesto do conjunto de aplicativo.

## <a name="how-to-create-an-app-installer-file"></a>Como criar um arquivo de Instalador de Aplicativo

Para distribuir seu conjunto relacionado como uma entidade, você deve criar um arquivo do Instalador de Aplicativo que contém os elementos que são exigidos pelo [Esquema appinstaller](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

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

### <a name="step-3-add-the-main-package-information"></a>Etapa 3: Adicione as informações do pacote principal

Se o pacote do aplicativo principal é um arquivo .msix ou. AppX, em seguida, use `<MainPackage>`, conforme mostrado abaixo. Certifique-se de incluir ProcessorArchitecture, conforme é obrigatório para não-pacote de pacotes.

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

Se o pacote do aplicativo principal for um .msixbundle ou. appxbundle ou um arquivo, em seguida, use o `<MainBundle>` em vez de `<MainPackage>` conforme mostrado abaixo. Para pacotes, ProcessorArchitecture não é necessária.

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

As informações no atributo `<MainBundle>` ou `<MainPackage>` devem corresponder ao elemento [Pacote/Identidade](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) no manifesto do lote de aplicativo ou manifesto do conjunto de aplicativo, respectivamente.

### <a name="step-4-add-the-optional-packages"></a>Etapa 4: Adicione os pacotes opcionais
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

### <a name="step-6-add-update-setting"></a>Etapa 6: Adicionar configuração de atualização

O arquivo do Instalador de Aplicativo também pode especificar a configuração de atualização para que os conjuntos relacionados possam ser atualizados automaticamente quando um arquivo mais recente do Instalador de Aplicativo for publicado. **<UpdateSettings>** é um elemento opcional. Dentro de **<UpdateSettings>** , a opção OnLaunch especifica que as verificações de atualização devem ser feitas na inicialização do aplicativo e HoursBetweenUpdateChecks = "12" especifica que uma verificação de atualização deve ser feita a cada 12 horas. Se HoursBetweenUpdateChecks não for especificado, o intervalo padrão usado para verificar se há atualizações será de 24 horas. Tipos de atualizações, como atualizações em segundo plano podem ser encontradas nas configurações de atualização adicionais [esquema](https://docs.microsoft.com/en-us/uwp/schemas/appinstallerschema/element-update-settings); Tipos adicionais de atualizações na inicialização, como atualizações com um prompt podem ser encontradas na OnLaunch [esquema](https://docs.microsoft.com/en-us/uwp/schemas/appinstallerschema/element-onlaunch)

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

Para todos os detalhes sobre o esquema XML, consulte [Referência de arquivo do Instalador de Aplicativo](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

> [!NOTE]
> O tipo de arquivo do instalador de aplicativo é novo no Windows 10, versão 1709 (o Windows 10 Fall Creators Update). Não há nenhum suporte para a implantação de aplicativos do Windows 10 usando um arquivo do instalador do aplicativo em versões anteriores do Windows 10. O **HoursBetweenUpdateChecks** elemento está disponível a partir do Windows 10, versão 1803.
