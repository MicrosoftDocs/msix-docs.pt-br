---
title: Pacotes opcionais com código executável
description: Saiba como usar o Visual Studio para criar um pacote opcional com código executável.
ms.date: 07/02/2019
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: Windows 10, msix, UWP, instalador de aplicativos, AppInstaller, Sideload, conjunto relacionado, pacotes opcionais
ms.localizationpriority: medium
ms.openlocfilehash: 6a3862f0c7eaa875444fc9fb99f7b54e1dbe7b5d
ms.sourcegitcommit: 769bbff22df0ab9e46e1306b4180bbec6a44daba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87538461"
---
# <a name="optional-packages-with-executable-code"></a>Pacotes opcionais com código executável
 
[Pacotes opcionais](optional-packages.md) com código executável são úteis para dividir um aplicativo grande ou complexo, ou para adicionar em um aplicativo que já foi publicado. Com o Visual Studio 2017, versão 15.7 e .NET Native 2.1, você pode carregar o código executável dos pacotes opcionais C++ e C#.

## <a name="prerequisites"></a>Pré-requisitos

- Visual Studio 2019 ou Visual Studio 2017 (versão 15,7 ou posterior)
- Windows 10, versão 1709 ou posterior
- Windows 10, versão 1709 SDK ou posterior

Para obter as ferramentas de desenvolvimento mais recentes, consulte [Downloads e ferramentas para Windows 10](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> Para enviar um aplicativo que usa pacotes opcionais e/ou conjuntos relacionados para a Store, você precisará de permissão. Os pacotes opcionais e os conjuntos relacionados podem ser usados para LOB (linha de negócios) ou aplicativos empresariais sem a permissão do Partner Center se não forem enviados para a loja. Consulte [Suporte do desenvolvedor Windows](https://developer.microsoft.com/windows/support) para obter permissão para enviar um aplicativo que usa pacotes opcionais e conjuntos relacionados.

> [!NOTE]
> Pacotes opcionais contendo código executável devem fazer parte de um [conjunto relacionado](optional-packages.md#related-sets). Você não poderá executar o código de um pacote opcional, a menos que ele faça parte de um conjunto relacionado.

## <a name="c-optional-packages-with-executable-code"></a>Pacotes opcionais do C++ com código executável

Para carregar o código de um pacote opcional do C++, consulte o repositório [OptionalPackageSample](https://github.com/AppInstaller/OptionalPackageSample) no GitHub. O [OptionalPackageDLL](https://github.com/AppInstaller/OptionalPackageSample/tree/master/OptionalPackageDLL) mostra como criar um projeto com um código que pode ser executado a partir do pacote principal. O projeto MyMainApp demonstra como [carregar o código](https://github.com/AppInstaller/OptionalPackageSample/blob/bf6b4915ff1f3b8abfdaacb1ad9e77184c49fe18/MyMainApp/MainPage.xaml.cpp#L182) a partir do arquivo OptionalPackageDLL.dll.

## <a name="c-optional-packages-with-executable-code"></a>Pacotes opcionais do C# com código executável

Para começar a criar um pacote opcional de código em C#, siga as etapas a seguir para configurar sua solução:

1. Crie um novo aplicativo UWP com a versão mínima definida com o SDK do Windows 10 Fall Creators Update (compilação 16299) ou superior.

2. Adicione um novo projeto de **Pacote opcional de código (Universal do Windows)** à solução. Certifique-se de que **Minimum Version** e **Target Version** correspondem ao do aplicativo principal.

3. Se você pretende enviar seus aplicativos para a Store, clique com botão direito do mouse em projetos e selecione **Store-> Associar aplicativo à Store...**

4. Abra o arquivo `Package.appxmanifest` do aplicativo principal e encontre o valor `Identity Name`. Anote esse valor para a próxima etapa.

5. Abra o arquivo `Package.appxmanifest` do pacote opcional do aplicativo e encontre o valor `uap3:MainAppPackageDependency Name`. Atualize o valor `uap3:MainAppPackageDependency Name` para corresponder ao valor de `Identity Name` do pacote de aplicativo principal da etapa anterior. 

    Veja um exemplo de `Identity`do `Package.appxmanifest` do aplicativo principal.
    ```XML
    <Identity Name="12345.MainAppProject" Publisher="CN=PublisherName" Version="1.0.0.0" />
    ```

    O `uap3:MainPackageDependency` do pacote opcional do aplicativo deve ser atualizado para corresponder a `Identity` do aplicativo principal.
    ```XML
    <uap3:MainPackageDependency Name="12345.MainAppProjectTest" />
    ```

6. Adicione um arquivo `Bundle.mapping.txt` ao aplicativo principal. Siga as etapas na seção [Conjuntos relacionados](optional-packages.md#related-sets) para criar um conjunto relacionado contendo ambos os aplicativos.

7. Compile o projeto do pacote opcional e vá até a pasta de Referência do pacote na saída da compilação encontrada em `..\[PathToOptionalPackageProject]\bin\[architecture]\[configuration]\Reference`. Observe que você pode escolher qualquer arquitetura no caminho para a pasta de Referência, pois o arquivo `.winmd` (etapa 8) não depende de arquitetura.

8. Adicione uma referência de projeto do aplicativo principal para o arquivo `.winmd` encontrado nessa pasta. O arquivo `.winmd`**deve** ser atualizado sempre que você alterar a área de superfície da API no projeto do pacote opcional. Essa referência fornece ao projeto de aplicativo principal as informações necessárias para a compilação.

9. No projeto do aplicativo principal, vá até as propriedades de compilação do projeto e selecione **Compilar com a cadeia de ferramenta .NET Native**. Atualmente, apenas a depuração no .NET Native é compatível com a criação do pacote opcional de código em C#. Vá até as propriedades de depuração do projeto e selecione **Implantar pacotes opcionais**. Isso garante que ambos os pacotes são sincronizados sempre que você implantar o projeto do aplicativo principal.

Depois de concluir essas etapas, você poderá adicionar código ao projeto de pacote opcional como se fosse um projeto de componente do WinRT gerenciado. Para acessar o código no projeto do aplicativo principal, chame os métodos públicos expostos no projeto do pacote opcional.
