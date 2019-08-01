---
Description: Este guia explica como configurar sua solução do Visual Studio para otimizar os binários de aplicativo com imagens nativas.
title: Otimize seus aplicativos de área de trabalho .NET com imagens nativas
ms.date: 07/29/2019
ms.topic: article
keywords: Windows 10, UWP, msix, compilador de imagem nativa
ms.localizationpriority: medium
ms.openlocfilehash: 6ce41bafb2debfb8c2d99135634ba30c91f4400a
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68685401"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>Otimize seus aplicativos de área de trabalho .NET com imagens nativas

Você pode melhorar o tempo de inicialização do seu aplicativo .NET Framework compilando previamente seus binários. Você pode usar essa tecnologia em aplicativos grandes que você empacota e distribui por meio do Microsoft Store. Em alguns casos, observamos uma melhoria de desempenho de 20%. Você pode aprender mais sobre essa tecnologia na [visão geral técnica](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md).

Lançamos o compilador de imagem nativa como um [pacote NuGet](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler). Você pode aplicar esse pacote a qualquer aplicativo .NET Framework que tenha como destino a .NET Framework versão 4.6.2 ou posterior. Esse pacote adiciona uma etapa de Build de postagem que inclui uma carga nativa a todos os binários usados pelo seu aplicativo. Essa carga otimizada será carregada quando o aplicativo for executado no .NET 4.7.2 e versões anteriores ainda carregará o código MSIL.

O [4.7.2 do .NET Framework](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) está incluído na [atualização do Windows 10 de abril de 2018](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/). Você também pode instalar essa versão do .NET Framework em computadores que executam o Windows 7 + e o Windows Server 2008 R2 +.

> [!IMPORTANT]
> Se você quiser produzir imagens nativas para seu aplicativo empacotado pelo projeto de empacotamento de aplicativos do Windows, certifique-se de definir a versão mínima da plataforma de destino do projeto para a atualização de aniversário do Windows.

## <a name="how-to-produce-native-images"></a>Como produzir imagens nativas

Siga estas instruções para configurar seus projetos.

1. Configurar a estrutura de destino como 4.6.2 ou superior

2. Configurar a plataforma de destino como x86 ou x64 

3. Adicione os pacotes NuGet.

4. Crie uma compilação de versão.

## <a name="configure-the-target-framework-as-462-or-above"></a>Configurar a estrutura de destino como 4.6.2 ou superior

Para configurar seu projeto para o destino .NET Framework 4.6.2, você precisará das ferramentas de desenvolvimento .NET Framework 4.6.2 ou mais recente. Essas ferramentas estão disponíveis por meio do instalador do Visual Studio como componentes opcionais na carga de trabalho de desenvolvimento de desktops .NET:

![Instalar ferramentas de desenvolvimento do .NET 4.6.2](images/install-4.6.2-devpack.png)

Como alternativa, você pode obter os pacotes de desenvolvedor do .NET de:[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>Configurar a plataforma de destino como x86 ou x64

O compilador de imagem nativa otimiza o código para uma determinada plataforma. Para usá-lo, você precisa configurar seu aplicativo para direcionar uma plataforma específica, como x86 ou x64.

Se você tiver vários projetos em sua solução, somente o projeto de ponto de entrada (provavelmente o projeto que produz um arquivo executável) precisará ser compilado como x86 ou x64. Os binários adicionais referenciados do projeto principal serão processados com a arquitetura especificada no projeto principal, mesmo se eles forem compilados como AnyCPU.

Para configurar seu projeto:

1. Clique com o botão direito do mouse em sua solução e selecione **Configuration Manager**.

2. Selecione **< novo.. >** no menu suspenso **plataforma** ao lado do nome do projeto que produz o arquivo executável.

3. Na caixa de diálogo **nova plataforma de projeto** , verifique se as **configurações de cópia da** lista suspensa estão definidas para **qualquer CPU**.

![Configurar x86](images/configure-x86.png)

Repita essa etapa para `Release/x64` se você quiser produzir binários x64.

>[!IMPORTANT]
> O compilador de imagem nativa não dá suporte à configuração AnyCPU.

## <a name="add-the-nuget-packages"></a>Adicionar os pacotes NuGet

O compilador de imagem nativa é fornecido como um pacote NuGet que você precisa adicionar ao projeto do Visual Studio que produz o arquivo executável. Normalmente, é o projeto Windows Forms ou WPF. Use este comando do PowerShell para fazer isso.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 1.0.0
```

## <a name="create-a-release-build"></a>Criar uma compilação de versão

O pacote NuGet configura o projeto para executar uma ferramenta adicional para Builds de versão. Essa ferramenta adiciona o código nativo aos mesmos binários.
Para verificar se a ferramenta processou os binários, você pode examinar a saída da compilação para ter certeza de que ela inclui uma mensagem como esta:

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>Perguntas Frequentes

**PERGUNTAS. Os novos binários funcionam em computadores sem .NET Framework 4.7.2?**

A. Os binários otimizados se beneficiarão das melhorias ao serem executados com .NET Framework 4.7.2. Os clientes que executam versões anteriores do .NET Framework carregarão o código MSIL não otimizado do binário.

**PERGUNTAS. Como posso fornecer comentários ou relatar problemas?**

A. Relate um problema usando a ferramenta de comentários no Visual Studio 2017. [Mais informações](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).

**PERGUNTAS. Qual é o impacto da adição da imagem nativa aos binários existentes?**

A. Os binários otimizados contêm o código gerenciado e nativo, tornando os arquivos finais maiores.

**PERGUNTAS. Posso liberar binários usando essa tecnologia?**

A. Esta versão inclui uma licença do Go Live que você pode usar atualmente.
