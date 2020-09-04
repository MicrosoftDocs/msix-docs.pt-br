---
description: Você pode usar o Azure Pipelines para criar builds automatizados para seu projeto MSIX.
title: Configure um pipeline da CI/CD para automatizar builds e implantações do MSIX
ms.date: 01/27/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 79b8e7472685f63104a8d5e1066a316a970ab8aa
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090624"
---
# <a name="set-up-a-cicd-pipeline-to-automate-your-msix-builds-and-deployments"></a>Configure um pipeline da CI/CD para automatizar builds e implantações do MSIX

Você pode usar o Azure Pipelines para criar builds automatizados para seu projeto MSIX. Este artigo mostra como fazer isso no Azure DevOps. Também mostraremos como realizar essas tarefas usando a linha de comando para que seja possível integrar com qualquer outro sistema de build.

## <a name="create-a-new-azure-pipeline"></a>Crie um novo pipeline do Azure

Comece [inscrevendo-se no Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-sign-up) caso ainda não tenha feito isso.

Em seguida, crie um pipeline que possa ser usado para criar seu código-fonte. Para obter um tutorial sobre como criar um pipeline para criar um repositório GitHub, consulte [Crie seu primeiro pipeline](/azure/devops/pipelines/get-started-yaml). O Azure Pipelines é compatível com os tipos de repositório listados [neste artigo](/azure/devops/pipelines/repos).

Para configurar o pipeline do build propriamente dito, navegue para o portal Azure DevOps em dev.azure.com/\<organization\> e crie um novo projeto. Se não tiver uma conta, crie uma gratuitamente. Depois de entrar e criar um projeto, você poderá enviar o código-fonte por push ao repositório Git configurado para você em https://\<organization\>@dev.azure.com/<organization\>/\<project\>/_git/\<project\> ou usar qualquer outro provedor, como o GitHub. Você poderá escolher a localização do repositório ao criar um pipeline no portal, basta primeiro clicar no botão **Pipelines**, depois em **Novo Pipeline**.

Na tela **Configurar** que aparecerá em seguida, você deverá selecionar a opção **Arquivo YAML existente do Azure Pipelines** e o caminho para o arquivo YAML verificado no repositório.

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Adicione o certificado do projeto à biblioteca Arquivos seguros

> [!NOTE]
>Se possível, você deve evitar enviar certificados para o seu repositório e o git os ignorará por padrão. Para gerenciar o tratamento seguro de arquivos confidenciais como certificados, o Azure DevOps oferece suporte ao recurso [arquivos seguros](/azure/devops/pipelines/library/secure-files?view=azure-devops).

Para carregar um certificado em seu build automatizado:

1. No Azure Pipelines, expanda **Pipelines** no painel de navegação e clique em **Biblioteca**.
2. Clique na guia **Arquivos seguros** e clique em **+ Arquivo seguro**.
3. Navegue até o arquivo de certificado e clique em **OK**.
4. Depois de carregar o certificado, selecione-o para visualizar as propriedades dele. Em **Permissões de pipeline**, habilite a opção **Autorizar para uso em todos os pipelines**.
5. Se a chave privada no certificado tiver uma senha, recomendamos que você a armazene no [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates) e a vincule a um [grupo de variáveis](/azure/devops/pipelines/library/variable-groups). Você pode usar a variável para acessar a senha pelo pipeline. Observe que uma senha é compatível apenas com a chave privada. No momento, não é possível usar um arquivo de certificado que seja, em si, protegido por senha.

> [!NOTE]
> A partir do Visual Studio 2019, um certificado temporário não é mais gerado nos projetos MSIX. Para criar ou exportar certificados, use os cmdlets do PowerShell descritos [neste artigo](../package/create-certificate-package-signing.md).


## <a name="configure-the-build-in-your-yaml-file"></a>Configure o Build no arquivo YAML

Os diferentes argumentos MSBuild que podem ser definidos para configurar o pipeline do build estão listados na tabela a seguir.

|**Argumento MSBuild**|**Valor**|**Descrição**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Define a pasta para armazenar os artefatos gerados. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Permite definir quais plataformas incluir no pacote. |
| AppxBundle | Sempre | Cria um .msixbundle/.appxbundle com os arquivos .msix/.appx da plataforma especificada. |
| UapAppxPackageBuildMode | StoreUpload | Gera o arquivo .msixupload/.appxupload e a pasta **_Test** para sideload. |
| UapAppxPackageBuildMode | CI | Gera somente o arquivo .msixupload/.appxupload. |
| UapAppxPackageBuildMode | SideloadOnly | Gera a pasta **_Test** apenas para sideload. |
| AppxPackageSigningEnabled | verdadeiro | Habilita a assinatura do pacote. |
| PackageCertificateThumbprint | Impressão digital do certificado | Esse valor **deve** corresponder à impressão digital no certificado de autenticação ou ser uma cadeia de caracteres vazia. |
| PackageCertificateKeyFile | Caminho | O caminho para o certificado a ser usado. Isso é recuperado pelos metadados do arquivo seguro. |
| PackageCertificatePassword | Senha | A senha da chave privada no certificado. Recomendamos que você armazene a senha no [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates) e vincule-a ao [grupo de variáveis](/azure/devops/pipelines/library/variable-groups). Você pode passar a variável para este argumento. |



Antes de criar o projeto de empacotamento da mesma forma que o assistente no Visual Studio usando a linha de comando MSBuild, o processo de build pode criar a versão do pacote MSIX sendo produzido editando o atributo Version no elemento Package no arquivo Package.appxmanifest. No Azure Pipelines, isso pode ser feito usando-se uma expressão para configurar uma variável de contador que é incrementada para cada build e um script do PowerShell que usa a classe System.Xml.Linq.XDocument no .NET para alterar o valor do atributo. 

Exemplo de arquivo YAML que define o Pipeline de Build do MSIX

```yml
pool: 
  vmImage: windows-2019
  
variables:
  buildPlatform: 'x86'
  buildConfiguration: 'release'
  major: 1
  minor: 0
  build: 0
  revision: $[counter('rev', 0)]
  
steps:
 - powershell: |
     # Update appxmanifest. This must be done before the build.
     [xml]$manifest= get-content ".\Msix\Package.appxmanifest"
     $manifest.Package.Identity.Version = "$(major).$(minor).$(build).$(revision)"    
     $manifest.save("Msix/Package.appxmanifest")
  displayName: 'Version Package Manifest'
  
- task: MSBuild@1
  inputs:
    solution: Msix/Msix.wapproj
    platform: $(buildPlatform)
    configuration: $(buildConfiguration)
    msbuildArguments: '/p:OutputPath=NonPackagedApp
     /p:UapAppxPackageBuildMode=SideLoadOnly  /p:AppxBundle=Never /p:AppxPackageOutput=$(Build.ArtifactStagingDirectory)\MsixDesktopApp.msix /p:AppxPackageSigningEnabled=false'
  displayName: 'Package the App'
  
- task: DownloadSecureFile@1
  inputs:
    secureFile: 'certificate.pfx'
  displayName: 'Download Secure PFX File'
  
- script: '"C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\signtool"
    sign /fd SHA256 /f $(Agent.TempDirectory)/certificate.pfx /p secret $(
    Build.ArtifactStagingDirectory)/MsixDesktopApp.msix'
  displayName: 'Sign MSIX Package'
  
- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
```




Abaixo está o detalhamento das diferentes tarefas de Build definidas no arquivo YAMl:

### <a name="configure-package-generation-properties"></a>Configure propriedades de geração de pacotes

A definição abaixo estabelece o diretório dos componentes do Build, a plataforma e se um pacote será criado. 

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\"
/p:UapAppxPackageBuildMode=SideLoadOnly
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Never
```

### <a name="configure-package-signing"></a>Configure a autenticação do pacote

Para autenticar o pacote MSIX (ou APPX), o pipeline precisa recuperar o certificado de autenticação. Para isso, adicione uma tarefa DownloadSecureFile antes da tarefa VSBuild.
Isso possibilitará o acesso ao certificado de autenticação por ```signingCert```.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

Em seguida, atualize a tarefa MSBuild para fazer referência ao certificado de autenticação:

```yml
- task: MSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" 
                  /p:AppxPackageDir="$(appxPackageDir)" 
                  /p:AppxBundle=Never 
                  p:UapAppxPackageBuildMode=SideLoadOnly 
                  /p:AppxPackageSigningEnabled=true
                  /p:PackageCertificateThumbprint="" 
                  /p:PackageCertificateKeyFile="$(signingCert.secureFilePath)"'
```

> [!NOTE]
> O argumento PackageCertificateThumbprint é definido intencionalmente como uma cadeia de caracteres vazia por precaução. Caso a impressão digital esteja definida no projeto, mas não corresponda ao certificado de autenticação, o build falhará com o erro: `Certificate does not match supplied signing thumbprint`.

### <a name="review-parameters"></a>Revise os parâmetros

Os parâmetros definidos com a sintaxe `$()` são variáveis ajustadas na definição do build e serão alterados em outros sistemas de build.


Para ver todas as variáveis predefinidas, consulte [Variáveis de build predefinidas](/azure/devops/pipelines/build/variables).

## <a name="configure-the-publish-build-artifacts-task"></a>Configure a tarefa Publicar Artefatos de Build

O pipeline MSIX padrão não salva os artefatos gerados. Para adicionar recursos de publicação à sua definição YAML, adicione as tarefas a seguir.

```yml
- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```

Você pode ver os artefatos gerados na opção **Artefatos** da página de resultados do build.


## <a name="appinstaller-file-for-non-store-distribution"></a>Arquivo AppInstaller para distribuição fora da Microsoft Store


Se estiver distribuindo o aplicativo fora da Microsoft Store, será possível utilizar o arquivo AppInstaller para instalação e atualizações do pacote.

Um arquivo .appinstaller que buscará arquivos atualizados em \\server\foo

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller xmlns="http://schemas.microsoft.com/appx/appinstaller/2018"
              Version="1.0.0.0"
              Uri="\\server\foo\MsixDesktopApp.appinstaller">
  <MainPackage Name="MyCompany.MySampleApp"
               Publisher="CN=MyCompany, O=MyCompany, L=Stockholm, S=N/A, C=Sweden"
               Version="1.0.0.0"
               Uri="\\server\foo\MsixDesktopApp.msix"
               ProcessorArchitecture="x86"/>
  <UpdateSettings>
    <OnLaunch HoursBetweenUpdateChecks="0" />
  </UpdateSettings>
</AppInstaller>
```

O elemento UpdateSettings é usado para informar ao sistema quando verificar por atualizações e se é necessário forçar o usuário a atualizar. A referência de esquema completa, incluindo os namespaces compatíveis com cada versão do Windows 10, pode ser encontrada nos documentos em bit.ly/2TGWnCR.

Se você adicionar o arquivo .appinstaller ao projeto de empacotamento e definir a propriedade Package Action dele como Content e a propriedade Copy to Output Directory como Copy if newer, será possível adicionar outra tarefa do PowerShell ao arquivo YAML que atualiza os atributos Version da raiz e os elementos MainPackage e salva o arquivo atualizado no diretório de teste:

```powershell
- powershell: |
  [Reflection.Assembly]::LoadWithPartialName("System.Xml.Linq")
  $doc = [System.Xml.Linq.XDocument]::Load(
    "$(Build.SourcesDirectory)/Msix/Package.appinstaller")
  $version = "$(major).$(minor).$(build).$(revision)"
  $doc.Root.Attribute("Version").Value = $version;
  $xName =
    [System.Xml.Linq.XName]
      "{http://schemas.microsoft.com/appx/appinstaller/2018}MainPackage"
  $doc.Root.Element($xName).Attribute("Version").Value = $version;
  $doc.Save("$(Build.ArtifactStagingDirectory)/MsixDesktopApp.appinstaller")
displayName: 'Version App Installer File'
```

Em seguida, você distribuiria o arquivo .appinstaller para os usuários finais e permitiria que eles clicassem duas vezes nele em vez de no arquivo .msix para instalar o aplicativo empacotado.

## <a name="continuous-deployment"></a>Implantação contínua

O próprio arquivo do instalador é um arquivo XML não compilado que pode ser editado depois do build, caso necessário. Isso facilita a utilização quando você implanta o software em diversos ambientes e quando deseja separar o pipeline do build pelo processo de lançamento.

Se você criar um pipeline de lançamento no Azure Portal usando o modelo “Trabalho vazio” e usar o pipeline de build recém-configurado como a origem do artefato a ser implantado, será possível adicionar a tarefa PowerShell ao estágio de lançamento para alterar dinamicamente os valores dos dois atributos Uri no arquivo .appinstaller para refletir a localização na qual o aplicativo foi publicado.

Uma tarefa de pipeline de lançamento que modifica os Uris no arquivo .appinstaller

```powershell
- powershell: |
  [Reflection.Assembly]::LoadWithPartialName("System.Xml.Linq")
  $fileShare = "\\filesharestorageccount.file.core.windows.net\myfileshare\"
  $localFilePath =
    "$(System.DefaultWorkingDirectory)\_MsixDesktopApp\drop\MsixDesktopApp.appinstaller"
  $doc = [System.Xml.Linq.XDocument]::Load("$localFilePath")
  $doc.Root.Attribute("Uri").Value = [string]::Format('{0}{1}', $fileShare,
    'MsixDesktopApp.appinstaller')
  $xName =
    [System.Xml.Linq.XName]"{http://schemas.microsoft.com/appx/appinstaller/2018}MainPackage"
  $doc.Root.Element($xName).Attribute("Uri").Value = [string]::Format('{0}{1}',
    $fileShare, 'MsixDesktopApp.appx')
  $doc.Save("$localFilePath")
displayName: 'Modify URIs in App Installer File'
```

Na tarefa acima, o URI é configurado no caminho UNC de um compartilhamento de arquivos do Azure. Como esse seria o local no qual o sistema operacional procuraria pelo pacote MSIX quando o aplicativo fosse instalado ou atualizado, também adicionei outro script de linha de comando ao pipeline de lançamento que primeiro mapeia o compartilhamento de arquivos na nuvem para a unidade Z:\ local no agente do build antes de usar o comando xcopy para copiar os arquivos .appinstaller e .msix para lá:

```yml
- script: |
  net use Z: \\filesharestorageccount.file.core.windows.net\myfileshare
    /u:AZURE\filesharestorageccount
    3PTYC+ociHIwNgCnyg7zsWoKBxRmkEc4Aew4FMzbpUl/
    dydo/3HVnl71XPe0uWxQcLddEUuq0fN8Ltcpc0LYeg==
  xcopy $(System.DefaultWorkingDirectory)\_MsixDesktopApp\drop Z:\ /Y
  displayName: 'Publish App Installer File and MSIX package'
```

Caso você hospede seu próprio Azure DevOps Server local, será possível publicar os arquivos em seu próprio compartilhamento de rede interno.

Se você optar por publicar em um servidor Web, será possível informar ao MSBuild para gerar um arquivo .appinstaller versionado e uma página HTML que contenha um link de download e algumas informações sobre o aplicativo empacotado fornecendo alguns argumentos adicionais no arquivo YAML:

```yml
- task: MSBuild@1
  inputs:
    solution: Msix/Msix.wapproj
    platform: $(buildPlatform)
    configuration: $(buildConfiguration)
    msbuildArguments: '/p:OutputPath=NonPackagedApp /p:UapAppxPackageBuildMode=SideLoadOnly  /p:AppxBundle=Never /p:GenerateAppInstallerFile=True
/p:AppInstallerUri=http://yourwebsite.com/packages/ /p:AppInstallerCheckForUpdateFrequency=OnApplicationRun /p:AppInstallerUpdateFrequency=1 /p:AppxPackageDir=$(Build.ArtifactStagingDirectory)/'
  displayName: 'Package the App'
  ```
  
O arquivo HTML gerado inclui um hiperlink prefixado com o esquema de ativação de protocolo ms-appinstaller que independe do navegador:

```html
<a href="ms-appinstaller:?source=
  http://yourwebsite.com/packages/Msix_x86.appinstaller ">Install App</a>
```

Se você configurar um pipeline de lançamento que publica o conteúdo da pasta de destino na intranet ou em qualquer outro site e o servidor Web oferecer suporte a solicitações de intervalo de bytes e estiver configurado corretamente, os usuários finais poderão usar esse link para instalar o aplicativo diretamente sem baixar o pacote MSIX primeiro.
