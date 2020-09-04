---
title: Usar propriedades personalizadas para extensões de aplicativo
description: Saiba como usar propriedades personalizadas para AppExtensions
ms.date: 02/06/2020
ms.topic: article
keywords: windows 10, msix, uwp, extensões
ms.localizationpriority: medium
ms.openlocfilehash: 4cf0edb1ae5a8c9e9c180fcec2922bbf60ccbfb5
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090124"
---
# <a name="using-custom-properties-for-app-extensions"></a>Usar propriedades personalizadas para extensões de aplicativo

As propriedades de extensão de aplicativo são um campo de metadados personalizados fornecido pelo desenvolvedor da extensão do aplicativo no manifesto do aplicativo. Os hosts de extensão do aplicativo podem ler esses metadados ao examinar extensões de aplicativo sem precisar carregar o conteúdo da extensão do aplicativo.

As propriedades são um recurso opcional, mas bastante útil. Há uma ampla variedade de metadados possíveis que você pode precisar ao criar uma plataforma de host de extensão de aplicativo, como versão, recursos, listas de tipos de arquivo compatíveis ou outros dados úteis que você deve saber antes de carregar uma extensão de aplicativo. Essas informações podem até mesmo determinar como você carrega uma extensão de aplicativo. Em vez de tentar prever todos os campos de que você pode precisar, as propriedades de extensão do aplicativo oferecem uma tela em branco para definir exatamente o que você precisa da forma mais adequada para seu aplicativo.

## <a name="advantages-of-app-extension-properties"></a>Vantagens das propriedades de extensão do aplicativo

Há dois motivos importantes pelos quais você deve aproveitar as propriedades de extensão do aplicativo:

* É uma forma fácil e segura de armazenar metadados importantes ou básicos sobre a plataforma de extensão do aplicativo sem precisar colocar as informações nos arquivos. Você pode usá-los para conceitos importantes como controle de versão, permissões e como imposição. Por exemplo, o aplicativo pode insistir que um campo `version` seja definido e esteja presente nas propriedades, além de ter comportamentos de carregamento diferentes com base no valor.

* Como as informações são armazenadas no manifesto do aplicativo, elas podem ser indexadas e disponibilizadas por uma API no futuro. Isso significa que você pode mostrá-las para o usuário na forma de balões, ou ter pesquisas de extensão de aplicativo mais refinadas por propriedades específicas sem precisar implantar e carregar a extensão primeiro.

## <a name="how-to-declare-properties"></a>Como declarar as propriedades

As propriedades são declaradas no arquivo Package.appxmanifest do pacote. Eles podem ser lidos no tempo de execução pelo host da extensão como um conjunto de propriedades. Como o host define a plataforma, fica a cargo do host comunicar aos desenvolvedores da extensão sobre quais propriedades estão disponíveis e devem ser colocadas na declaração `AppExtension`.

> [!NOTE]
> O designer do manifesto no Visual Studio não é compatível com a capacidade de definir propriedades. Você deve editar o Package.appxmanifest diretamente para definir as propriedades.

Para declarar as propriedades, coloque-as no elemento `<uap3:Properties/>` na declaração `<uap3:AppExtension>`. Este é um exemplo de declaração `<uap3:AppExtension>` do Microsoft Edge que usa propriedades compatíveis com o Edge.

```xml
<uap3:AppExtension Name="com.microsoft.edge.extension" Id="FirstExtension" PublicFolder="Extension" DisplayName="MyExtension">
  <uap3:Properties>
    <Capabilities>
      <Capability Name="websiteContent" />
      <Capability Name="websiteInfo" />
      <Capability Name="browserWebRequest" />
      <Capability Name="browserStorage" />
    </Capabilities>
  </uap3:Properties>
</uap3:AppExtension>
```

O Edge definiu um valor de propriedade conhecido de `Capabilities` com uma lista declarada de recursos de extensão. Enquanto host, é possível oferecer suporte às propriedades desejadas nas extensões do aplicativo. Como elas são um conjunto de propriedades, também é possível ter propriedades aninhadas. É útil ter uma propriedade de versão raiz que possa ser usada caso você altere os formatos das extensões no futuro. Não colocamos `Version` intencionalmente como um atributo de extensões de aplicativo para que você não fique restrito artificialmente ao uso da semântica de controle de versão. Em vez disso, criamos propriedades nas quais a versão pode ser um dos muitos atributos personalizados, na forma e formato que você quiser, e processados à seu critério.

## <a name="how-to-use-properties"></a>Como usar propriedades

Suponha que você tenha uma propriedade simples em uma extensão de aplicativo que descreva a versão, como a seguir.

```xml
<uap3:Properties>
    <Version>1.0.0.0</Version>
</uap3:Properties>
```

Para obter esses dados no tempo de execução, basta chamar [GetExtensionPropertiesAsync()](/uwp/api/windows.applicationmodel.appextensions.appextension.getextensionpropertiesasync) nas extensões de aplicativo.

```csharp
string extensionVersion = "Unknown";
var properties = await ext.GetExtensionPropertiesAsync() as PropertySet;
if (properties != null)
{
    if (properties.ContainsKey("Version"))
    {
        PropertySet versionProperty = properties["Version"] as PropertySet;
        extensionVersion = versionProperty["#text"].ToString();
    }
}
```