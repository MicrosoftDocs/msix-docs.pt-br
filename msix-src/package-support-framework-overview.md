---
author: mcleanbyron
Description: A estrutura de suporte de pacote ajuda a corrigir problemas que impedem que seu aplicativo da área de trabalho seja executado em um contêiner de MSIX.
Search.Product: eADQiWindows 10XVcnh
title: Estrutura de suporte do pacote
ms.author: mcleans
ms.date: 09/05/2018
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2eccc27d426b3192112770db91c7ab65745738ff
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900858"
---
# <a name="package-support-framework"></a>Estrutura de suporte do pacote

A estrutura de suporte do pacote é um kit de software livre que ajuda você a aplicar correções para o seu aplicativo win32 quando você não tem acesso ao código-fonte, para que ele pode ser executado em um contêiner de MSIX. A estrutura de suporte de pacote ajuda seu aplicativo siga as práticas recomendadas do ambiente moderno do tempo de execução.

Para criar a estrutura de suporte do pacote, aproveitamos os [desvios](https://www.microsoft.com/en-us/research/project/detours) tecnologia que é uma estrutura de software livre desenvolvida pela Microsoft Research (MSR) e ajuda com o redirecionamento de API e interceptação.

Essa estrutura é um software livre, leve, e você pode usá-lo para solucionar problemas de aplicativo rapidamente. Ele também oferece a oportunidade de consultar com a comunidade em todo o mundo e criar com base em investimentos de outras pessoas.

Para obter um guia passo a passo, consulte [tempo de execução de aplicar correções a um pacote MSIX usando a estrutura de suporte do pacote](https://docs.microsoft.com/windows/uwp/porting/package-support-framework).

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>Uma olhada rápida dentro a estrutura de suporte do pacote

A estrutura de suporte do pacote contém um executável, uma DLL do Gerenciador de tempo de execução e um conjunto de correções de tempo de execução.

![Estrutura de suporte do pacote](images/package-support-framework.png)

Veja como funciona. Você criará um arquivo de configuração que especifica as correções que você deseja aplicar ao seu aplicativo. Em seguida, você modificará o pacote para apontar para o arquivo executável do iniciador de estrutura de suporte do pacote (PSF).

Quando os usuários iniciam seu aplicativo, o iniciador de estrutura de suporte do pacote é o primeiro executável que é executado. Ele lê o arquivo de configuração e injeta as correções de tempo de execução e a DLL do Gerenciador de tempo de execução do processo do aplicativo. O Gerenciador de tempo de execução se aplica a correção quando ela é necessária para o aplicativo seja executado dentro de um contêiner MSIX.

![Injeção de DLL de estrutura de suporte de pacote](images/package-support-framework-2.png)

## <a name="how-to-use-the-package-support-framework"></a>Como usar a estrutura de suporte do pacote

Depois de criar um pacote para seu aplicativo, instalar e executá-lo e observar seu comportamento. Você pode receber mensagens de erro que podem ajudá-lo a identificar um problema de compatibilidade. Você também pode usar [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) para identificar problemas.

Depois de encontrar um problema, você pode verificar nossos [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/) página uma correção. Se você encontrar um, você pode aplicá-lo ao seu pacote. Nossos [guia passo a passo](https://docs.microsoft.com/windows/uwp/porting/package-support-framework) mostra como fazer isso. Ela também mostrará como usar o depurador do Visual Studio para depurar seu aplicativo e verificar se a correção está funcionando e que ela resolveu o problema de compatibilidade.

Se você não encontrar uma correção de tempo de execução que resolve o problema, você pode criar um. Para fazer o que, você identificará qual função chamadas falham quando seu aplicativo é executado em um contêiner de MSIX. Em seguida, você pode criar funções de substituição que você gostaria que o Gerenciador de tempo de execução para chamar em vez disso. Isso lhe dá uma oportunidade para substituir a implementação de uma função com o comportamento está de acordo com as regras do ambiente moderno do tempo de execução.

## <a name="get-started-with-the-package-support-framework"></a>Comece com a estrutura de suporte do pacote

Se você estiver pronto para começar a usar a estrutura de suporte do pacote para solucionar problemas de compatibilidade, consulte nosso guia passo a passo em [tempo de execução de aplicar correções a um pacote MSIX usando a estrutura de suporte do pacote](https://docs.microsoft.com/windows/uwp/porting/package-support-framework).
