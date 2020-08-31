---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Criação de pacotes opcionais e conjunto relacionado
description: Os pacotes opcionais contêm conteúdo que pode ser integrado com um pacote principal. Eles são úteis para o conteúdo para download (DLC) e outros cenários.
ms.date: 07/02/2019
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: Windows 10, msix, UWP, pacotes opcionais, conjunto relacionado, extensão do pacote, Visual Studio
ms.localizationpriority: medium
ms.openlocfilehash: c896edc6b2bc835bd6b37917675dbd466ec256ae
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091084"
---
# <a name="optional-packages-and-related-set-authoring"></a>Criação de pacotes opcionais e conjunto relacionado

Os pacotes opcionais contêm conteúdo que pode ser integrado com um pacote principal. Estes são úteis para o conteúdo para download (DLC), dividindo um aplicativo grande para restrições de tamanho ou para enviar qualquer conteúdo adicional para separado do seu aplicativo original. Para obter mais informações sobre pacotes opcionais, consulte [postagem de blog: estender seu aplicativo usando pacotes opcionais](/archive/blogs/appinstaller/uwpoptionalpackages).

Os conjuntos relacionados são uma extensão de pacotes opcionais. Os conjuntos relacionados permitem que você aplique um conjunto estrito de versões em pacotes principais e opcionais. Os conjuntos relacionados podem ter editores diferentes do aplicativo principal se ele for implantado fora do repositório. Para obter mais informações sobre conjuntos relacionados, consulte [postagem no blog: ferramentas para criar um conjunto relacionado](/archive/blogs/appinstaller/tooling-to-create-a-related-set).

Pacotes opcionais e conjuntos relacionados todos são executados dentro do contêiner MSIX do aplicativo principal.

## <a name="prerequisites"></a>Pré-requisitos

- Visual Studio 2019 ou Visual Studio 2017 (versão 15,1 ou posterior)
- Windows 10, versão 1703 ou posterior
- Windows 10, versão 1703 SDK ou posterior

Para obter todas as ferramentas de desenvolvimento mais recentes, consulte [Downloads e ferramentas para Windows 10](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> Para enviar um aplicativo que usa pacotes opcionais e/ou conjuntos relacionados para o Microsoft Store, você precisará de permissão. Os pacotes opcionais e os conjuntos relacionados podem ser usados para LOB (linha de negócios) ou aplicativos empresariais sem a permissão do Partner Center se não forem enviados para a loja. Consulte [Suporte do desenvolvedor Windows](https://developer.microsoft.com/windows/support) para obter permissão para enviar um aplicativo que usa pacotes opcionais e conjuntos relacionados.

## <a name="code-sample"></a>Exemplo de código

Enquanto você está lendo este artigo, recomenda-se que você acompanhe o [exemplo de código do pacote opcional](https://github.com/AppInstaller/OptionalPackageSample) no GitHub para obter um conhecimento prático de como os pacotes opcionais e conjuntos relacionados funcionam no Visual Studio.

## <a name="optional-packages"></a>Pacotes opcionais

Para criar um pacote opcional no Visual Studio, você precisará:

1. Verifique se a **versão mínima da plataforma de destino** do aplicativo está definida como: 10.0.15063.0 ou superior.
2. Em seu projeto **pacote principal**, abra o arquivo `Package.appxmanifest`. Navegue até a guia "Pacotes" e anote seu **nome de família do pacote**, que é tudo antes do caractere "_".
3. Em seu projeto **pacote opcional**, clique com botão direito em `Package.appxmanifest` e selecione **Abrir com > Editor XML (texto)**.
4. Localize o elemento `<Dependencies>` no arquivo. Adicione o seguinte e substitua `[MainPackageDependency]` pelo **nome da família de pacotes** da etapa 2. Isso especifica que seu **pacote opcional** depende seu **pacote principal**.
    ```XML
    <uap3:MainPackageDependency Name="[MainPackageDependency]"/>
    ```

Depois de ter suas dependências de pacote configuradas das etapas 1 a 4, você pode continuar desenvolvendo como faria normalmente. Para obter mais informações, consulte [postagem no blog: criar seu primeiro pacote opcional](/archive/blogs/appinstaller/build-your-first-optional-package).

O Visual Studio pode ser configurado para implementar novamente seu pacote principal sempre que você implantar um pacote opcional. Para definir a dependência de compilação no Visual Studio, você deve:

1. Clique com o botão direito no projeto de pacote opcional e selecione **Dependências da compilação > Dependências do projeto...**
2. Verifique se o projeto do pacote principal e selecione "OK". 

Agora, toda vez que você pressionar F5 ou criar um projeto de pacote opcional, o Visual Studio irá construir o projeto do pacote principal primeiro. Isso garante que seu projeto principal e projetos opcionais estejam em sincronia.

## <a name="related-sets"></a>Conjuntos relacionados

Um conjunto relacionado consiste em um pacote principal e um pacote opcional que são rigidamente acoplados por meio de metadados que são especificados no arquivo. appxbundle ou. msixbundle do pacote principal. Esses metadados vinculam o pacote principal ao pacote opcional (usando o nome do arquivo. appxbundle + versão) e o pacote opcional para o pacote principal (usando o nome independente da versão). O Visual Studio ajuda você a obter os metadados corretos em seus arquivos. 

O controle de versão de pacotes em um conjunto relacionado é sincronizado de uma maneira que não permitirá que a versão mais recente de qualquer pacote seja usada até que todos os pacotes do conjunto relacionado (especificados pela versão no pacote principal) estejam instalados. Os pacotes são atendidos de forma independente, mas os pacotes especificados no conjunto podem não ser usados até que todos tenham sido atualizados. Para obter mais informações sobre conjuntos relacionados, consulte [postagem no blog: ferramentas para criar um conjunto relacionado](/archive/blogs/appinstaller/tooling-to-create-a-related-set).

Para configurar a solução do seu aplicativo para conjuntos relacionados, use as seguintes etapas:

1. Clique com botão direito do projeto do pacote principal, selecione **Adicionar > Novo Item...**
2. Na janela, pesquise os modelos instalados para ". txt" e adicione um novo arquivo de texto.
    > [!IMPORTANT]
    > O novo arquivo de texto deve ser nomeado: `Bundle.Mapping.txt`.
3. No `Bundle.Mapping.txt` arquivo, insira a cadeia de caracteres "[OptionalProjects]" seguida pelos caminhos relativos para seus projetos de pacote opcionais. Veja aqui um exemplo de arquivo `Bundle.Mapping.txt`:
    ```syntax
    [OptionalProjects]
    "..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
    "..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"
    ```

Quando sua solução estiver configurada dessa forma, o Visual Studio criará um [manifesto de pacote](/uwp/schemas/bundlemanifestschema/bundle-manifest) chamado AppxBundleManifest.xml para o pacote principal com todos os metadados necessários para conjuntos relacionados. 

Observe que, assim como os pacotes opcionais, um `Bundle.Mapping.txt` arquivo para conjuntos relacionados só funcionará no Windows 10, versão 1703 ou superior. Além disso, a versão mínima da plataforma de destino do seu aplicativo deve ser definida como 10.0.15063.0 ou superior.

## <a name="removing-optional-packages"></a>Removendo pacotes opcionais

Os usuários podem acessar seu aplicativo de **configurações** e remover os pacotes opcionais. Da mesma forma, os desenvolvedores podem usar o [RemoveOptionalPackageAsync](/uwp/api/Windows.ApplicationModel.PackageCatalog) para remover uma lista de pacotes opcionais. 

```csharp
PackageCatalog catalog = PackageCatalog.OpenForCurrentPackage();
List<string> optionalList = new List<string>();
optionalList.Add("FabrikamAgeAnalysis_kwpnjs8c36mz0");
    
// Warn user that application will be restarted. 
var result = await catalog.RemoveOptionalPackagesAsync(optionalList);
if (result.ExtendedError != null)
{
    throw removalResult.ExtendedError;
}
```
> [!NOTE]
> No caso de um conjunto relacionado, a plataforma precisará reiniciar o aplicativo principal para finalizar a remoção para evitar situações em que o aplicativo tem conteúdo carregado do pacote que está sendo removido. Os aplicativos devem notificar os usuários de que o aplicativo precisará ser reiniciado antes que o aplicativo chame a API.

Se o pacote opcional for somente conteúdo, o desenvolvedor deverá informar explicitamente à plataforma que o pacote que está prestes a remover não está em uso pelo aplicativo antes que o desenvolvedor remova o pacote opcional. Isso também permite que o desenvolvedor remova o pacote sem uma reinicialização.

## <a name="known-issues"></a>Problemas conhecidos

A depuração de um projeto opcional de conjunto relacionado não é atualmente suportada no Visual Studio. Para resolver este problema, você pode implantar e iniciar a ativação (Ctrl + F5) e anexar manualmente o depurador a um processo. Para anexar o depurador, vá no menu "Depurar" no Visual Studio, selecione "Anexar ao Processo ..." e anexe o depurador ao **processo principal do aplicativo**.

## <a name="related-articles"></a>Artigos relacionados

* [Postagem no blog: Estenda seu aplicativo usando pacotes opcionais](/archive/blogs/appinstaller/uwpoptionalpackages)
* [Postagem no blog: criar seu primeiro pacote opcional](/archive/blogs/appinstaller/build-your-first-optional-package)
* [Postagem do blog: carregando código de um pacote opcional](/archive/blogs/appinstaller/loading-code-from-an-optional-package)
* [Postagem no blog: ferramentas para criar um conjunto relacionado](/archive/blogs/appinstaller/tooling-to-create-a-related-set)