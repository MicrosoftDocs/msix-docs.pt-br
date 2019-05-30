---
Description: Este guia explica como configurar sua solução do Visual Studio para otimizar os binários do aplicativo com as imagens nativas.
title: Otimize seus aplicativos de área de trabalho do .NET com imagens nativas
ms.date: 06/11/2018
ms.topic: article
keywords: Windows 10, o compilador de imagem nativa
ms.localizationpriority: medium
ms.openlocfilehash: 1fa6d50f6dbb7e16d00a656f8fd931595a4ce69e
ms.sourcegitcommit: c3bdc2150bba942dc95811746c7a0f14ce54fbc9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985496"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>Otimize seus aplicativos de área de trabalho do .NET com imagens nativas

> [!NOTE]
> Algumas informações estão relacionadas a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não faz nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.

Você pode melhorar o tempo de inicialização do aplicativo do .NET Framework, pré-compilando os binários. Você pode usar essa tecnologia em aplicativos grandes que você empacotar e distribuir por meio do Microsoft Store. Em alguns casos, observamos uma melhoria de desempenho de 20%. Você pode aprender mais sobre essa tecnologia na [visão geral técnica](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md).

Lançamos uma versão prévia do compilador de imagem nativa como um [pacote do NuGet](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler). Você pode aplicar esse pacote a qualquer aplicativo .NET Framework que tem como alvo o .NET Framework versão 4.6.2 ou posterior. Esse pacote adiciona uma etapa de compilação de post que inclui uma carga nativa para todos os binários usados pelo seu aplicativo. Essa carga otimizada seja carregada quando o aplicativo é executado no .NET 4.7.2 e posterior enquanto as versões anteriores ainda carregará o código MSIL.

O [.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) está incluído na [Windows 10 de abril de 2018 update](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/). Você também pode instalar esta versão do .NET Framework em PCs que executam o Windows 7 + e Windows Server 2008 R2 +.

> [!IMPORTANT]
> Se você quiser produzir imagens nativas para seu aplicativo empacotado pelo projeto de empacotamento de aplicativos do Windows, certifique-se de definir a versão mínima da plataforma de destino do projeto para a atualização de aniversário do Windows.

## <a name="how-to-produce-native-images"></a>Como gerar imagens nativas

Siga estas instruções para configurar seus projetos.

1. Configurar a estrutura de destino como 4.6.2 ou acima

2. Configurar a plataforma de destino como x86 ou x64 

3. Adicione os pacotes do NuGet.

4. Crie um Build de versão.

## <a name="configure-the-target-framework-as-462-or-above"></a>Configurar a estrutura de destino como 4.6.2 ou acima

Para configurar seu projeto para o destino do .NET Framework 4.6.2, você precisará as ferramentas de desenvolvimento do .NET Framework 4.6.2 ou mais recente. Essas ferramentas estão disponíveis por meio do instalador do Visual Studio como componentes opcionais sob a carga de trabalho de desenvolvimento para desktop do .NET:

![Instalar o .NET 4.6.2 ferramentas de desenvolvimento](images/install-4.6.2-devpack.png)

Como alternativa, você pode obter os pacotes de desenvolvedor do .NET da: [https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>Configurar a plataforma de destino como x86 ou x64

O compilador de imagem nativa otimiza o código para uma determinada plataforma. Para usá-lo, você precisará configurar seu aplicativo para direcionar uma plataforma específica, como x86 ou x64.

Se você tiver vários projetos em sua solução, apenas o entrada ponto projeto (provavelmente o projeto que produz um arquivo executável) tem que ser compilado como x86 ou x64. Binários adicionais referenciados no projeto principal serão processados com a arquitetura especificada no projeto principal, mesmo se eles são compilados como AnyCPU.

Para configurar seu projeto:

1. Sua solução com o botão direito e, em seguida, selecione **Configuration Manager**.

2. Selecione **< novo... >** no **plataforma** menu suspenso ao lado do nome do projeto que produz o arquivo executável.

3. No **nova plataforma de projeto** diálogo caixa, certifique-se de que o **copiar configurações de** lista suspensa é definida como **qualquer CPU**.

![Configurar x86](images/configure-x86.png)

Repita essa etapa para `Release/x64` se você quiser produzir x64 binários.

>[!IMPORTANT]
> Não há suporte para a configuração de AnyCPU pelo compilador de imagem nativa.

## <a name="add-the-nuget-packages"></a>Adicione os pacotes NuGet

O compilador de imagem nativa é fornecido como um pacote do NuGet que você precisa adicionar ao projeto do Visual Studio que produz o arquivo executável. Isso é normalmente seu projeto Windows Forms ou WPF. Use este comando do PowerShell para fazer isso.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> Os pacotes de visualização são publicados em NuGet.org como não listados. Você não encontrá-los procurando no NuGet.org ou usando a UI do Gerenciador de pacotes no Visual Studio. No entanto, você pode instalá-los no Console do Gerenciador de pacotes e quando você restaurar de um computador diferente. Faremos os pacotes totalmente acessível ao publicar a primeira versão de não-preview.

## <a name="create-a-release-build"></a>Criar um Build de versão

O pacote do NuGet configura o projeto para executar uma ferramenta adicional para builds de versão. Essa ferramenta adiciona o código nativo para os mesmos binários.
Para verificar se a ferramenta processou os binários, você pode examinar a saída da compilação para verificar se que ele inclui uma mensagem como esta:

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>Perguntas frequentes

**P. Os novos binários funcionam em computadores sem o .NET Framework 4.7.2?**

A. Binários otimizados se beneficiarão das melhorias de durante a execução do .NET Framework 4.7.2. Clientes que executam versões anteriores do .NET framework carregará o código MSIL não otimizada do binário.

**P. Como fornecer comentários ou relatar problemas?**

A. Relate um problema usando a ferramenta de comentários no Visual Studio 2017. [Obter mais informações](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).

**P. O que é o impacto de adicionar a imagem nativa para binários existentes?**

A. Os binários otimizados contêm o código gerenciado e nativo, tornando os arquivos finais maior.

**P. Eu liberar binários usando essa tecnologia?**

A. Esta versão inclui uma licença do Go Live que você pode usar hoje mesmo.
