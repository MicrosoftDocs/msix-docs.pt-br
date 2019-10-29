---
Description: O Package Support Framework ajuda você a corrigir problemas que impedem que seu aplicativo da área de trabalho seja executado em um contêiner MSIX.
title: PSF (estrutura de suporte do pacote)
ms.date: 09/05/2018
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: fa8cc955d44ddf2a3db5900e9f054a23e0037242
ms.sourcegitcommit: f47c140e2eb410c2748be7912955f43e7adaa8f9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72776466"
---
# <a name="package-support-framework"></a>PSF (estrutura de suporte do pacote)

O Package Support Framework é um kit de software livre que ajuda você a aplicar correções ao seu aplicativo Win32 quando você não tem acesso ao software livre, de modo que ele possa ser executado em um contêiner MSIX. O Package Support Framework ajuda seu aplicativo a seguir as melhores práticas do ambiente moderno do tempo de execução.

Estes estão alguns exemplos comuns em que o Package Support Framework pode ser útil:

* Quando iniciado, seu aplicativo não consegue localizar algumas DLLs. Talvez você precise definir seu diretório de trabalho atual. Você pode descobrir o diretório de trabalho atual necessário no atalho original, antes da conversão em MSIX.
* O aplicativo faz gravações na pasta de instalação. Você normalmente percebe isso por causa de erros de "Acesso Negado" no [Process Monitor](https://docs.microsoft.com/windows/msix/psf/package-support-framework).
* Ao ser iniciado, seu aplicativo precisa passar parâmetros ao executável. Você pode saber mais sobre como identificar esse problema [aqui](package-support-framework.md#identify-packaged-application-compatibility-issues) e sobre as configurações disponíveis [aqui](https://github.com/microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher).

Para criar o Package Support Framework, aproveitamos a tecnologia [Detours](https://www.microsoft.com/en-us/research/project/detours), que é uma estrutura de software livre desenvolvida pela MSR (Microsoft Research) e que ajuda com o redirecionamento de API e a vinculação.

Essa estrutura é software livre, leve e você pode usá-la para solucionar problemas do aplicativo rapidamente. Ela também oferece a oportunidade de consultar a comunidade em todo o mundo e aproveitar os investimentos de outras pessoas.

Para obter um guia passo a passo, confira [Aplicar correções em tempo de execução em um pacote MSIX usando o Package Support Framework](https://docs.microsoft.com/windows/uwp/porting/package-support-framework).

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>Uma visão rápida do Package Support Framework

O Package Support Framework contém um executável, uma DLL do gerenciador de tempo de execução e um conjunto de correções em tempo de execução.

![PSF (estrutura de suporte do pacote)](images/package-support-framework.png)

Veja como funciona. Você criará um arquivo de configuração que especifica as correções que deseja aplicar ao seu aplicativo. Em seguida, você modificará o pacote para apontar para o arquivo executável do iniciador do PSF (Package Support Framework).

Quando os usuários iniciarem seu aplicativo, o iniciador do Package Support Framework será o primeiro executável que será executado. Ele lê o arquivo de configuração e injeta as correções em tempo de execução e a DLL do gerenciador de tempo de execução no processo do aplicativo. O gerenciador de tempo de execução aplica a correção quando ela é necessária para que o aplicativo seja executado em um contêiner MSIX.

![Injeção de DLL do Package Support Framework](images/package-support-framework-2.png)

## <a name="get-started-using-the-package-support-framework"></a>Introdução ao uso do Package Support Framework

Depois de criar um pacote para seu aplicativo, instale e execute-o e observe seu comportamento. Você poderá receber mensagens de erro que podem ajudá-lo a identificar um problema de compatibilidade. Use também o [Monitor do Processo](https://docs.microsoft.com/sysinternals/downloads/procmon) para identificar problemas.

Depois de encontrar um problema, confira nossa página do [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/) para obter uma correção. Caso encontre uma, aplique-a ao seu pacote. Nosso [guia passo a passo](https://docs.microsoft.com/windows/uwp/porting/package-support-framework) mostra como fazer isso. Ele também mostrará como usar o depurador do Visual Studio para depurar seu aplicativo e verificar se a correção está funcionando e se ela resolveu o problema de compatibilidade.

Caso não encontre uma correção de tempo de execução que resolva o problema, [crie uma](package-support-framework.md#create-a-runtime-fix). Para fazer isso, você identificará quais chamadas de função falham quando seu aplicativo é executado em um contêiner MSIX. Em seguida, você poderá criar funções de substituição que você deseja que sejam chamadas pelo gerenciador de tempo de execução. Isso oferecerá uma oportunidade de substituir a implementação de uma função por um comportamento que esteja de acordo com as regras do ambiente moderno do tempo de execução.

Também é possível usar o Package Support Framework para executar scripts a fim de personalizar dinamicamente um aplicativo para o ambiente do usuário. Para obter mais informações, consulte [este artigo](run-scripts-with-package-support-framework.md).

## <a name="limitations"></a>Limitações

O Package Support Framework não é compatível com as substituições do registro. Ele foi criado para resolver problemas de tempo de execução.

## <a name="data-and-telemetry"></a>Dados e telemetria

O Package Support Framework inclui a telemetria que coleta dados de uso e os envia para a Microsoft para ajudar a melhorar nossos produtos e serviços. Leia a [política de privacidade da Microsoft para saber mais](https://privacy.microsoft.com/en-US/privacystatement). No entanto, os dados serão coletados somente quando as duas condições a seguir forem atendidas:

* Os binários do Package Support Framework são usados do [pacote NuGet](https://www.nuget.org/packages?q=packagesupportframework) em um computador com Windows 10.
* O usuário habilitou a coleta de dados no computador.

O pacote NuGet contém binários assinados e coletará dados de uso do computador. A telemetria não é coletada quando os binários são criados localmente clonando o repositório ou baixando os binários diretamente.
