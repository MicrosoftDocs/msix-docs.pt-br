---
Description: Este guia explica como configurar a solução do Visual Studio para otimizar os binários do aplicativo com imagens nativas.
title: Otimize os aplicativos de área de trabalho .NET com imagens nativas
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10, uwp, msix, compilador de imagem nativa
ms.localizationpriority: medium
ms.openlocfilehash: ebc2c7351fef0856d83529e55ba4ebf012aaaeed
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2020
ms.locfileid: "77073086"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>Otimize os aplicativos de área de trabalho .NET com imagens nativas

Você pode melhorar o tempo de inicialização do aplicativo .NET Framework compilando previamente os binários. Você pode usar essa tecnologia em aplicativos grandes, empacotando-os e distribuindo-os pela Microsoft Store. Em alguns casos, observamos uma melhoria de desempenho de 20%. Você pode saber mais sobre essa tecnologia na [visão geral técnica](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md).

Lançamos o compilador de imagem nativa como um [pacote NuGet](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler). Você pode aplicar esse pacote a qualquer aplicativo .NET Framework que tenha como destino o .NET Framework versão 4.6.2 ou posterior. Esse pacote adiciona uma etapa pós-build que inclui uma carga nativa a todos os binários usados pelo aplicativo. Essa carga otimizada será carregada quando o aplicativo for executado no .NET 4.7.2 ou posterior; versões anteriores ainda carregarão o código MSIL.

O [.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) está incluído na [atualização de abril de 2018 do Windows 10](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/). Você também pode instalar essa versão do .NET Framework em computadores que executam o Windows 7+ e o Windows Server 2008 R2+.

> [!IMPORTANT]
> Se você quiser produzir imagens nativas para o aplicativo empacotado pelo Projeto de Empacotamento de Aplicativo do Windows, assegure-se de definir a versão mínima da plataforma de destino do projeto para a Atualização de Aniversário do Windows.

## <a name="how-to-produce-native-images"></a>Como produzir imagens nativas

Siga essas instruções para configurar os projetos.

1. Configure a estrutura de destino como 4.6.2 ou superior

2. Configurar a plataforma de destino como x86 ou x64 

3. Adicionar os pacotes NuGet.

4. Crie um build de lançamento.

## <a name="configure-the-target-framework-as-462-or-above"></a>Configure a estrutura de destino como 4.6.2 ou superior

Para configurar o projeto para dar preferência ao .NET Framework 4.6.2, será necessário ter as ferramentas de desenvolvimento do .NET Framework 4.6.2 ou mais recentes. Essas ferramentas estão disponíveis pelo instalador do Visual Studio como componentes opcionais na carga de trabalho de desenvolvimento de área de trabalho .NET:

![Instalar ferramentas de desenvolvimento .NET 4.6.2](images/install-4.6.2-devpack.png)

Como alternativa, é possível obter os pacotes de desenvolvedor do .NET em: [https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>Configure a plataforma de destino como x86 ou x64

O compilador de imagem nativa otimiza o código para uma determinada plataforma. Para usá-lo, você precisa configurar o aplicativo para visar uma plataforma específica, como x86 ou x64.

Se você tiver vários projetos em sua solução, somente o projeto de ponto de entrada (provavelmente o projeto que produz um arquivo executável) precisará ser compilado como x86 ou x64. Os binários adicionais referenciados do projeto principal serão processados com a arquitetura especificada no projeto principal, mesmo se eles forem compilados como AnyCPU.

Para configurar seu projeto:

1. Clique com o botão direito do mouse na solução e selecione **Gerenciador de Configurações**.

2. Selecione **<Novo...>** no menu suspenso **Plataforma** ao lado do nome do projeto que produz o arquivo executável.

3. Na caixa de diálogo **Nova Plataforma de Projeto**, verifique se a lista suspensa **Copiar Configurações de** está definida como **AnyCPU**.

![Configure o x86](images/configure-x86.png)

Repita essa etapa para `Release/x64` se desejar produzir binários x64.

>[!IMPORTANT]
> A configuração AnyCPU não é compatível com o compilador de imagem nativa.

## <a name="add-the-nuget-packages"></a>Adicione os pacotes NuGet

O compilador de imagem nativa é fornecido como um pacote NuGet que você precisa adicionar ao projeto do Visual Studio que produz o arquivo executável. Isso normalmente é projeto WPF ou Windows Forms. Use esse comando do PowerShell para fazer isso.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 1.0.0
```

## <a name="create-a-release-build"></a>Crie um build de lançamento

O pacote NuGet configura o projeto para executar uma ferramenta adicional nos builds de lançamento. Essa ferramenta adiciona o código nativo aos mesmos binários.
Para verificar se a ferramenta processou os binários, é possível rever a saída do build para garantir que ele inclua uma mensagem como esta:

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

A compilação de imagem nativa pode ser acionada em builds que não sejam de lançamento ao definir a propriedade `NgenR2R` como `true` no arquivo do projeto.

## <a name="faq"></a>Perguntas frequentes

**P. Os novos binários funcionam em computadores sem o .NET Framework 4.7.2?**

A. Os binários otimizados vão se beneficiar das melhorias se forem executados com o .NET Framework 4.7.2. Os clientes com versões anteriores do .NET framework vão carregar o código MSIL não otimizado do binário.

**P. Como posso fornecer comentários ou relatar problemas?**

A. Informe um problema usando a ferramenta Comentários no Visual Studio 2017. [Mais informações](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).

**P. Qual é o impacto de adicionar a imagem nativa aos binários existentes?**

A. Os binários otimizados contêm o código nativo e gerenciado, tornando os arquivos finais maiores.

**P. Posso lançar binários usando essa tecnologia?**

A. Essa versão inclui uma licença do Go Live que você pode usar hoje mesmo.
