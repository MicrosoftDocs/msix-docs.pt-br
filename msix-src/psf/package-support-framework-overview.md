---
Description: O Package Support Framework ajuda você a corrigir problemas que impedem que seu aplicativo da área de trabalho seja executado em um contêiner MSIX.
title: PSF (estrutura de suporte do pacote)
ms.date: 09/05/2018
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 98e837669feda0147ad9465330907c930132b43d
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829112"
---
# <a name="package-support-framework"></a>PSF (estrutura de suporte do pacote)

O Package Support Framework é um kit de software livre que ajuda você a aplicar correções ao seu aplicativo Win32 quando você não tem acesso ao software livre, de modo que ele possa ser executado em um contêiner MSIX. O Package Support Framework ajuda seu aplicativo a seguir as melhores práticas do ambiente moderno do tempo de execução.

Para criar o Package Support Framework, aproveitamos a tecnologia [Detours](https://www.microsoft.com/en-us/research/project/detours), que é uma estrutura de software livre desenvolvida pela MSR (Microsoft Research) e que ajuda com o redirecionamento de API e a vinculação.

Essa estrutura é software livre, leve e você pode usá-la para solucionar problemas do aplicativo rapidamente. Ela também oferece a oportunidade de consultar a comunidade em todo o mundo e aproveitar os investimentos de outras pessoas.

Para obter um guia passo a passo, confira [Aplicar correções em tempo de execução em um pacote MSIX usando o Package Support Framework](https://docs.microsoft.com/windows/uwp/porting/package-support-framework).

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>Uma visão rápida do Package Support Framework

O Package Support Framework contém um executável, uma DLL do gerenciador de tempo de execução e um conjunto de correções em tempo de execução.

![PSF (estrutura de suporte do pacote)](images/package-support-framework.png)

Veja como funciona. Você criará um arquivo de configuração que especifica as correções que deseja aplicar ao seu aplicativo. Em seguida, você modificará o pacote para apontar para o arquivo executável do iniciador do PSF (Package Support Framework).

Quando os usuários iniciarem seu aplicativo, o iniciador do Package Support Framework será o primeiro executável que será executado. Ele lê o arquivo de configuração e injeta as correções em tempo de execução e a DLL do gerenciador de tempo de execução no processo do aplicativo. O gerenciador de tempo de execução aplica a correção quando ela é necessária para que o aplicativo seja executado em um contêiner MSIX.

![Injeção de DLL do Package Support Framework](images/package-support-framework-2.png)

## <a name="how-to-use-the-package-support-framework"></a>Como usar o Package Support Framework

Depois de criar um pacote para seu aplicativo, instale e execute-o e observe seu comportamento. Você poderá receber mensagens de erro que podem ajudá-lo a identificar um problema de compatibilidade. Use também o [Monitor do Processo](https://docs.microsoft.com/sysinternals/downloads/procmon) para identificar problemas.

Depois de encontrar um problema, confira nossa página do [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/) para obter uma correção. Caso encontre uma, aplique-a ao seu pacote. Nosso [Guia passo a passo](https://docs.microsoft.com/windows/uwp/porting/package-support-framework) mostra como fazer isso. Ele também mostrará como usar o depurador do Visual Studio para depurar seu aplicativo e verificar se a correção está funcionando e se ela resolveu o problema de compatibilidade.

Caso não encontre uma correção em tempo de execução que resolva o problema, crie uma. Para fazer isso, você identificará quais chamadas de função falham quando seu aplicativo é executado em um contêiner MSIX. Em seguida, você poderá criar funções de substituição que você deseja que sejam chamadas pelo gerenciador de tempo de execução. Isso oferecerá uma oportunidade de substituir a implementação de uma função por um comportamento que esteja de acordo com as regras do ambiente moderno do tempo de execução.

## <a name="get-started-with-the-package-support-framework"></a>Introdução ao Package Support Framework

Se você estiver pronto para começar a usar o Package Support Framework para solucionar problemas de compatibilidade, confira nosso guia passo a passo em [Aplicar correções em tempo de execução a um pacote MSIX usando o Package Support Framework](https://docs.microsoft.com/windows/uwp/porting/package-support-framework).
